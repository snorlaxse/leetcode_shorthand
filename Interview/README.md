# Module


| 题目 | 链接 | 核心公式 |
| --- | --- | --- |
| Multi-Head Attention<br>多头注意力 | [Link](https://github.com/snorlaxse/leetcode_shorthand/blob/master/Interview/Module.md#%E5%A4%9A%E5%A4%B4%E6%B3%A8%E6%84%8F%E5%8A%9B) |$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h)W^O$<br>$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$ |
| Scaled Dot-Product Attention<br>缩放点积注意力| [Link](https://github.com/snorlaxse/leetcode_shorthand/blob/master/Interview/Module.md#%E7%BC%A9%E6%94%BE%E7%82%B9%E7%A7%AF%E6%B3%A8%E6%84%8F%E5%8A%9B) |$$\text{index} = b \times (H \times L \times D) + h \times (L \times D) + i \times D + j$$ |


# Math
| 题目 | 链接 | 核心公式 |
| --- | --- | --- |
| 判断点是否在三角形内部 | [Link](https://github.com/snorlaxse/leetcode_shorthand/blob/master/Interview/Math.md#%E5%88%A4%E6%96%AD%E7%82%B9%E6%98%AF%E5%90%A6%E5%9C%A8%E4%B8%89%E8%A7%92%E5%BD%A2%E5%86%85%E9%83%A8) | 叉乘符号法 (Cross Product): $cross\\_product = x_1 y_2 - x_2 y_1$ |
| 平方根 | [Link](https://github.com/snorlaxse/leetcode_shorthand/blob/master/Interview/Math.md#%E5%B9%B3%E6%96%B9%E6%A0%B9) | 1. 牛顿迭代法 (Newton's Method): $y_{n+1} = \frac{1}{2}(y_n + \frac{x}{y_n})$<br>2. 二分查找法 (Binary Search)<br>3. Quake III 快速平方根倒数 |
