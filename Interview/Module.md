### 多头注意力

核心公式：
$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h)W^O$$

其中每一路 Head 的计算公式为：
$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

-----
**核心步骤拆解**
1. Linear Projection: 把输入映射到 $Q, K, V$。
2. Split Heads: 将隐藏层维度 $d_{model}$ 切分为 $h$ 个头。
3. Scaled Dot-Product: 计算注意力权重并加权。
4. Concatenation: 合并所有头并经过最后的线性层。
----
Pytorch 手撕版：重点在于 view 和 transpose 的配合
```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import math

class MultiHeadAttention(nn.Module):
    def __init__(self, d_model: int, num_heads: int, dropout: float = 0.1):
        super().__init__()
        assert d_model % num_heads == 0, "d_model 必须能被 num_heads 整除"
        
        self.d_model = d_model
        self.num_heads = num_heads
        self.d_k = d_model // num_heads
        
        # 定义四个线性变换
        self.w_q = nn.Linear(d_model, d_model)
        self.w_k = nn.Linear(d_model, d_model)
        self.w_v = nn.Linear(d_model, d_model)
        # 输出层线性变换
        self.w_o = nn.Linear(d_model, d_model)
        
        self.dropout = nn.Dropout(dropout)

    def forward(self, q, k, v, mask=None):
        batch_size = q.size(0)
        
        # 1. 线性映射得到 Q/K/V & 拆成多头
        # (batch, seq_len, d_model) -> (batch, n_heads, seq_len, d_k)
        q = self.w_q(q).view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        k = self.w_k(k).view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        v = self.w_v(v).view(batch_size, -1, self.num_heads, self.d_k).transpose(1, 2)
        
        # 2. 缩放点积注意力
        # scores shape: (batch, n_heads, seq_len, seq_len)
        attn_scores = torch.matmul(q, k.transpose(-2, -1)) / math.sqrt(self.d_k)
        
        if mask is not None:
            # mask 为 0 的地方填充负无穷
            attn_scores = attn_scores.masked_fill(mask == 0, -1e9)
        
        attn_probs = F.softmax(attn_scores, dim=-1)
        attn = self.dropout(attn_probs)
        
        # 3. Multiply by V 加权和
        # output shape: (batch, n_heads, seq_len, d_k)
        output = torch.matmul(attn, v)
        
        # 4. Concatenate heads 拼回 d_model 维度
        # (batch, n_heads, seq_len, d_k) -> (batch, seq_len, d_model)
        output = output.transpose(1, 2).contiguous().view(batch_size, -1, self.d_model)
        
        # 5. 输出线性映射
        return self.w_o(output) # (batch, seq_len_q, d_model)
        
if __name__ == "__main__":
    # 简单测试，方便手撕后快速自检
    bs, seq_len, d_model, num_heads = 2, 4, 8, 2
    x = torch.randn(bs, seq_len, d_model)
    mha = MultiHeadAttention(d_model=d_model, num_heads=num_heads)
    y = mha(x)
    print(y.shape)  # 期望: (2, 4, 8)
```
#### 考察细节

- 为什么要除以 $\sqrt{d_k}$？<br>
防止点积结果过大，导致 Softmax 进入梯度平缓区（梯度消失），从而使模型难以训练。

- 为什么要在 `view` 之后做 `transpose(1, 2)`？<br>
为了让 `seq_len` 和 `d_k` 在最后两个维度，方便直接进行矩阵乘法（Batch Matrix Multiplication）。

- `contiguous()` 的作用是什么？<br>
`transpose` 后的 Tensor 在内存中不再连续。`view` 操作要求 Tensor 必须是连续的，所以需要调用此方法重新开辟内存复制一遍。
- Mask 的形状通常是什么样的？<br>
对于 Encoder，通常是 `(batch, 1, 1, seq_len)`；对于 Decoder（因果掩码），则是下三角矩阵。

---
### 缩放点积注意力
核心挑战：维度索引映射

对于形状为 $(B, H, L, D)$ 的张量，位置 $(b, h, i, j)$ 对应的线性偏移量为：
$$\text{index} = b \times (H \times L \times D) + h \times (L \times D) + i \times D + j$$

---
基于 LibTorch 的实现：处理 张量乘法、维度转置、缩放以及 Mask
```C++
#include <torch/torch.h>
#include <cmath>

using namespace torch;

Tensor scaled_dot_product_attention(Tensor q, Tensor k, Tensor v, Tensor mask = Tensor()) {
    // 1. 计算 d_k (最后一个维度的大小)
    int64_t d_k = q.size(-1);

    // 2. 计算评分矩阵 scores = Q * K^T
    // q shape: (batch, n_heads, seq_len, d_k)
    // k.transpose(-2, -1) shape: (batch, n_heads, d_k, seq_len)
    Tensor scores = torch::matmul(q, k.transpose(-2, -1));

    // 3. 缩放 (Scaling)
    scores = scores / std::sqrt(static_cast<double>(d_k));

    // 4. 应用 Mask (如果有)
    if (mask.defined()) {
        // 将 mask 为 0 的地方设为一个极小的负数 (趋于负无穷)
        scores = scores.masked_fill(mask == 0, -1e9);
    }

    // 5. Softmax 归一化得到权重
    Tensor weights = torch::softmax(scores, dim=-1);

    // 6. 权重乘 V 得到最终输出
    // weights shape: (batch, n_heads, seq_len, seq_len)
    // v shape: (batch, n_heads, seq_len, d_k)
    return torch::matmul(weights, v);
}
```
试官其实更想看对 维度（Dimension）的控制。写完代码后，主动给面试官 Trace 一遍 Shape 变化：<br/>
- $Q: [B, H, L, D]$<br/>
- $K^T: [B, H, D, L]$<br/>
- $Scores: [B, H, L, L]$<br/>
- $V: [B, H, L, D]$<br/>
- $Output: [B, H, L, D]$<br/>

#### 考察细节
1. `transpose(-2, -1)` vs `view`: <br/>在计算 $QK^T$ 时，必须使用 transpose 交换最后两个维度。千万不要用 `view` 或 `reshape`，因为它们会打乱内存顺序而非进行数学上的矩阵转置。


2. `mask.defined()`:<br/>在 LibTorch 中，检查一个 Tensor 是否为空（即 Python 中的 `None`）要用 `.defined()`。

3. 内存连续性 (`contiguous`):<br/>如果之后要对结果进行 `view` 操作（比如在 Multi-Head Attention 合并头时），记得在 `transpose` 后调用 `.contiguous()`，否则会报错。

4. 数据类型:<br/>注意 `std::sqrt` 的输入需要是浮点型，所以代码中做了 `static_cast<double>`

---

C++ 手撕版
```C++
#include <iostream>
#include <vector>
#include <cmath>
#include <algorithm>

struct Tensor {
    std::vector<float> data;
    int B, H, L, D; // Batch, Heads, SeqLen, Dim

    Tensor(int b, int h, int l, int d) : B(b), H(h), L(l), D(d) {
        data.resize(b * h * l * d, 0.0f);
    }

    // 辅助函数：获取指定位置的引用
    float& at(int b, int h, int i, int j) {
        return data[b * (H * L * D) + h * (L * D) + i * D + j];
    }
};

// 1. Softmax 实现 (针对最后一行)
void softmax(float* row, int size) {
    float max_val = *std::max_element(row, row + size); // 防止指数溢出
    float sum = 0.0f;
    for (int i = 0; i < size; ++i) {
        row[i] = std::exp(row[i] - max_val);
        sum += row[i];
    }
    for (int i = 0; i < size; ++i) row[i] /= sum;
}

// 2. Scaled Dot-Product Attention 核心逻辑
Tensor scaled_dot_product(Tensor& Q, Tensor& K, Tensor& V, float scale) {
    int B = Q.B, H = Q.H, L = Q.L, D = Q.D;
    Tensor output(B, H, L, D);
    
    // 每一个 Batch 和 Head 独立计算
    for (int b = 0; b < B; ++b) {
        for (int h = 0; h < H; ++h) {
            
            // 计算 QK^T 并存储在临时得分矩阵 (L x L)
            std::vector<std::vector<float>> scores(L, std::vector<float>(L, 0.0f));
            for (int i = 0; i < L; ++i) {
                for (int j = 0; j < L; ++j) {
                    float dot = 0.0f;
                    for (int k = 0; k < D; ++k) {
                        dot += Q.at(b, h, i, k) * K.at(b, h, j, k); // K^T 相当于索引交换
                    }
                    scores[i][j] = dot * scale;
                }
                // 对每一行进行 Softmax
                softmax(scores[i].data(), L);
            }

            // 权重矩阵与 V 相乘 (L x L) * (L x D) -> (L x D)
            for (int i = 0; i < L; ++i) {
                for (int j = 0; j < D; ++j) {
                    float sum = 0.0f;
                    for (int k = 0; k < L; ++k) {
                        sum += scores[i][k] * V.at(b, h, k, j);
                    }
                    output.at(b, h, i, j) = sum;
                }
            }
        }
    }
    return output;
}
```
写纯 C++ 时，边写边解释：

1、分块思想：按 Batch 和 Head 拆分任务。

2、数值稳定性：Softmax 减去 max_val（减法在指数运算中变除法，防止 exp 溢出）。

3、索引计算：准确写出 4D 转 1D 的公式。

#### 优化点
如果你写出了上面的 $O(N^3)$ 嵌套循环，面试官接下来会问如何优化。

A. 内存访问局部性 (Cache Locality)

在计算矩阵乘法时，K.at(b, h, j, k) 的访问在最内层循环是不连续的（跨步访问）。
- 优化： 提前对 $K$ 进行转置，使内层循环变成两个连续内存的累加，极大提升 CPU 缓存命中率。

B. SIMD 指令集
- 可以使用 AVX/SSE 指令集对点积运算进行并行化，一次处理 8 个 float。

C. 为什么 $QK^T$ 的 $K$ 不需要真的转置？
- 在代码里，我们通过 K.at(b, h, j, k) 访问，其中 $j$ 是序列维度，$k$ 是特征维度。在数学上，这已经完成了 $K^T$ 的效果。如果面试官问“内存里怎么做最快”，你应该回答：“先将 $K$ 转置为 $(B, H, D, L)$ 存储，确保最内层遍历是连续的。”


