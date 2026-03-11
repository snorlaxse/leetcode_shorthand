### 判断点是否在三角形内部

#### 叉乘符号法 (Cross Product)
**原理：**

如果点 $P$ 在三角形 $ABC$ 内部，那么沿同一方向（如顺时针或逆时针）行走时，$P$ 始终位于每条边 $AB, BC, CA$ 的同一侧。

在 2D 空间中，向量 $\vec{AB}$ 与 $\vec{AP}$ 的叉乘结果的符号（正或负）决定了 $P$ 相对于 $AB$ 的位置。

**公式：**

对于向量 $\vec{u}(x_1, y_1)$ 和 $\vec{v}(x_2, y_2)$，其 2D 叉乘（标量形式）为：$$\text{cross\_product} = x_1 y_2 - x_2 y_1$$


**纯 C++ 实现**
```C++
#include <iostream>

struct Point {
    double x, y;
};

class TriangleChecker {
public:
    // 计算向量 (p1-p2) 与 (p1-p3) 的叉乘
    // 实际上是计算向量 P1P2 和 P1P3 的外积
    double crossProduct(Point p1, Point p2, Point p3) {
        return (p2.x - p1.x) * (p3.y - p1.y) - (p2.y - p1.y) * (p3.x - p1.x);
    }

    bool isPointInTriangle(Point A, Point B, Point C, Point P) {
        // 计算 P 相对于三条边的位置
        double d1 = crossProduct(A, B, P);
        double d2 = crossProduct(B, C, P);
        double d3 = crossProduct(C, A, P);

        // 检查是否所有叉乘结果符号相同
        bool has_neg = (d1 < 0) || (d2 < 0) || (d3 < 0);
        bool has_pos = (d1 > 0) || (d2 > 0) || (d3 > 0);

        // 如果既没有负号也没有正号（即 d1=d2=d3=0），说明三角形退化成一个点
        // 如果只有一个方向有值，说明在内部；如果同时有正有负，说明在外部
        return !(has_neg && has_pos);
    }
};

int main() {
    Point a = {0, 0}, b = {5, 0}, c = {0, 5};
    Point p1 = {1, 1}; // 内部
    Point p2 = {10, 10}; // 外部

    TriangleChecker checker;
    std::cout << "P1 in triangle: " << (checker.isPointInTriangle(a, b, c, p1) ? "Yes" : "No") << std::endl;
    std::cout << "P2 in triangle: " << (checker.isPointInTriangle(a, b, c, p2) ? "Yes" : "No") << std::endl;

    return 0;
}
```
**建议**：面试时首选叉乘法，因为它代码量最小且逻辑最直观

##### 考察细节
- 这种方法能处理点在边上的情况吗？<br>
答：可以。在上面的实现中，如果点在边上，对应的叉乘结果为 0。!(has_neg && has_pos) 的逻辑依然成立。如果你想严格排除边上的点，可以改为要求三个 d 必须严格大于 0（或严格小于 0）。

- 还有其他解法吗？<br>
1. 面积法 (Area Method)：计算 $S_{PAB} + S_{PBC} + S_{PCA}$ 是否等于总面积 $S_{ABC}$。
- 缺点：浮点数运算较多，且无法区分点在三角形外但投影在内部的情况（虽然在这个特定问题中由于面积累加会失效，但精度控制更难）。
2. 重心坐标法 (Barycentric Coordinates)：将 $P$ 表示为 $P = uA + vB + wC$，其中 $u+v+w=1$。
- 判定条件：如果 $0 \le u, v, w \le 1$，则点在内部。这是图形学中最专业的做法，适合在着色器（Shader）中使用。

- 浮点数精度如何处理？<br>
答：在实际工程中，不能直接用 d == 0。应该定义一个极小值 EPS = 1e-9，判断 abs(d) < EPS。

### 平方根

一种是要求整数结果（二分法），另一种是要求高精度浮点数结果（牛顿迭代法）。面试官通常更青睐后者，因为它是数学美感与工程实践的结合。

#### 牛顿迭代法 (Newton's Method)
这是求解平方根最快、最优雅的方法。<br>
核心思想：求 $\sqrt{x}$ 相当于求函数 $f(y) = y^2 - x$ 的零点。<br>
根据牛顿迭代公式：$y_{n+1} = y_n - \frac{f(y_n)}{f'(y_n)}$，代入得：
$$y_{n+1} = \frac{1}{2}(y_n + \frac{x}{y_n})$$

C++实现
```C++
#include <iostream>
#include <cmath>

double mySqrtNewton(double x) {
    if (x < 0) return -1; // 处理负数
    if (x == 0) return 0;

    double res = x; // 初始猜测值
    double eps = 1e-9; // 精度要求

    while (true) {
        double next_res = 0.5 * (res + x / res);
        if (std::abs(res - next_res) < eps) break;
        res = next_res;
    }
    return res;
}
```

#### 二分查找法 (Binary Search)
如果面试的是基础算法岗，二分法是必考的。它比较稳健，但收敛速度比牛顿迭代慢。

C++ 实现 (浮点数版)
```C++
double mySqrtBinary(double x) {
    if (x < 0) return -1;
    if (x < 1) { // 处理 0 < x < 1 的情况，此时根比 x 大
        double l = x, r = 1.0;
        for(int i = 0; i < 100; ++i) { // 迭代固定次数或判断 eps
            double mid = l + (r - l) / 2;
            if (mid * mid < x) l = mid;
            else r = mid;
        }
        return l;
    }
    
    double l = 0, r = x;
    while (r - l > 1e-9) {
        double mid = l + (r - l) / 2;
        if (mid * mid < x) l = mid;
        else r = mid;
    }
    return l;
}
```

#### 面试官的杀手锏：Quake III 快速平方根倒数
如果面的是图形学或者底层开发，提到这个算法会直接让面试官眼前一亮。

核心：利用 IEEE 754 浮点数表示法的特性，通过一个“魔数” 0x5f3759df 快速估算。

```C++
float Q_rsqrt(float number) {
    long i;
    float x2, y;
    const float threehalfs = 1.5F;

    x2 = number * 0.5F;
    y  = number;
    i  = * ( long * ) &y;                       // 邪恶的浮点数位转换
    i  = 0x5f3759df - ( i >> 1 );               // 什么是魔数？
    y  = * ( float * ) &i;
    y  = y * ( threehalfs - ( x2 * y * y ) );   // 第 1 次牛顿迭代
    // y  = y * ( threehalfs - ( x2 * y * y ) ); // 第 2 次迭代（可选）

    return 1.0f / y; // 得到结果
}
```

#### 总结与避坑

- 边界条件：一定要考虑 $x < 0$（抛异常或返回 NaN）、$x = 0$、$x < 1$。
- 精度控制：不要用 res * res == x，要用 abs(res * res - x) < eps。
- 初值选择：牛顿迭代中，初值选 $x$ 没问题，但在某些极致优化中，初值会选得更接近 $\sqrt{x}$。
- 收敛速度：牛顿迭代是平方收敛（每一步有效数字翻倍），二分查找是线性收敛。