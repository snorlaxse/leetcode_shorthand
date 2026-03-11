# 🚀 LeetCode Hot 100 核心题目速查表

## 哈希表

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **哈希表** | 1. 两数之和 | [Link](https://leetcode.cn/problems/two-sum/) | **哈希映射**：遍历时存入 `{目标值-当前值: 下标}`，实现 $O(1)$ 查找。 |
| **哈希表** | 49. 字母异位词分组 | [Link](https://leetcode.cn/problems/group-anagrams/) | **哈希分组**：排序后的字符串/计数签名作为 key。 |
| **哈希表** | 128. 最长连续序列 | [Link](https://leetcode.cn/problems/longest-consecutive-sequence/) | **哈希集合**：只从“序列起点”(x-1 不在集合) 开始往后数。 |

## 双指针

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **双指针** | 11. 盛最多水的容器 | [Link](https://leetcode.cn/problems/container-with-most-water/) | **左右指针**：每次移动短板（面积上界由短板决定）。 |
| **双指针** | 15. 三数之和 | [Link](https://leetcode.cn/problems/3sum/) | **排序+双指针**：固定一个数，剩余用夹逼；跳过重复。 |
| **双指针** | 42. 接雨水 | [Link](https://leetcode.cn/problems/trapping-rain-water/) | **双指针/单调栈**：双指针维护左右最高；或栈找“凹槽”计算水量。 |
| **双指针** | 75. 颜色分类 | [Link](https://leetcode.cn/problems/sort-colors/) | **三指针**：荷兰国旗，`0` 放左、`2` 放右、`1` 留中间。 |
| **双指针** | 283. 移动零 | [Link](https://leetcode.cn/problems/move-zeroes/) | **快慢指针**：慢指针写非零，最后补零（或原地交换）。 |
| **双指针** | 287. 寻找重复数 | [Link](https://leetcode.cn/problems/find-the-duplicate-number/) | **快慢指针找环**：把数组视作指针映射，重复数是入环点。 |

## 滑动窗口

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **滑动窗口** | 3. 无重复字符的最长子串 | [Link](https://leetcode.cn/problems/longest-substring-without-repeating-characters/) | **双指针/窗口**：右扩遇重复则左缩；用哈希记录最近位置/计数。 |
| **滑动窗口** | 76. 最小覆盖子串 | [Link](https://leetcode.cn/problems/minimum-window-substring/) | **窗口计数**：扩张满足后尽量收缩；维护 `need/have`。 |
| **滑动窗口** | 438. 找到字符串中所有字母异位词 | [Link](https://leetcode.cn/problems/find-all-anagrams-in-a-string/) | **固定窗口计数**：窗口长度为 p，维护差分计数或匹配次数。 |

## 字符串

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **字符串** | 5. 最长回文子串 | [Link](https://leetcode.cn/problems/longest-palindromic-substring/) | **中心扩展**：枚举中心（奇/偶），向两侧扩展取最长。 |
| **字符串** | 647. 回文子串 | [Link](https://leetcode.cn/problems/palindromic-substrings/) | **中心扩展**：以每个中心扩展计数；或 DP 记录回文。 |

## 数组

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **数组** | 31. 下一个排列 | [Link](https://leetcode.cn/problems/next-permutation/) | **字典序技巧**：从右找首个下降点，交换后对后缀升序（反转）。 |
| **数组** | 169. 多数元素 | [Link](https://leetcode.cn/problems/majority-element/) | **摩尔投票**：抵消不同元素，剩余候选即多数。 |
| **数组** | 238. 除自身以外数组的乘积 | [Link](https://leetcode.cn/problems/product-of-array-except-self/) | **前后缀乘积**：`ans[i]=prefix[i]*suffix[i]`，O(1) 额外空间可行。 |
| **数组** | 448. 找到所有数组中消失的数字 | [Link](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/) | **原地标记**：把 `nums[abs(x)-1]` 置负，未被标记的位置即缺失。 |
| **数组** | 581. 最短无序连续子数组 | [Link](https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/) | **一次扫描**：从左维护最大、从右维护最小，定位左右边界。 |

## 子数组

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **子数组** | 560. 和为 K 的子数组 | [Link](https://leetcode.cn/problems/subarray-sum-equals-k/) | **前缀和+哈希**：计数 `preSum-k` 出现次数，线性统计。 |
## 矩阵

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **矩阵** | 48. 旋转图像 | [Link](https://leetcode.cn/problems/rotate-image/) | **原地变换**：先转置再每行反转；或按环逐层旋转。 |
| **矩阵** | 240. 搜索二维矩阵 II | [Link](https://leetcode.cn/problems/search-a-2d-matrix-ii/) | **从右上/左下走**：大了左移，小了下移，O(m+n)。 |

## 链表

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **链表** | 2. 两数相加 | [Link](https://leetcode.cn/problems/add-two-numbers/) | **模拟竖式加法**：逐位相加+进位，边算边建新链表。 |
| **链表** | 19. 删除链表的倒数第 N 个结点 | [Link](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/) | **快慢指针**：快指针先走 N 步，再同步走到尾，慢指针定位待删前驱。 |
| **链表** | 21. 合并两个有序链表 | [Link](https://leetcode.cn/problems/merge-two-sorted-lists/) | **双指针归并**：新建空头节点，其后仅依次连接较小的值（双指针依次比较） |
| **链表** | 23. 合并 K 个升序链表 | [Link](https://leetcode.cn/problems/merge-k-sorted-lists/) | **小根堆**：堆存各链表头结点，反复取最小并推进该链表。 |
| **链表** | 141. 环形链表 | [Link](https://leetcode.cn/problems/linked-list-cycle/) | **快慢指针**：有环则必相遇。 |
| **链表** | 142. 环形链表 II | [Link](https://leetcode.cn/problems/linked-list-cycle-ii/) | **数学推导**：相遇后一指针回头，同速前进再相遇于入环点。 |
| **链表** | 148. 排序链表 | [Link](https://leetcode.cn/problems/sort-list/) | **归并排序**：快慢找中点拆分，递归合并；O(n log n)。 |
| **链表** | 160. 相交链表 | [Link](https://leetcode.cn/problems/intersection-of-two-linked-lists/) | **双指针换头**：A+B 与 B+A 走法长度相同，最终相遇于交点/Null。 |
| **链表** | 206. 反转链表 | [Link](https://leetcode.cn/problems/reverse-linked-list/) | **迭代三指针**：`prev/curr/next` 逐个反转指向。 |
| **链表** | 234. 回文链表 | [Link](https://leetcode.cn/problems/palindrome-linked-list/) | **快慢+反转后半**：比较两半，再可选恢复链表。 |

## 二叉树

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **二叉树** | 94. 二叉树的中序遍历 | [Link](https://leetcode.cn/problems/binary-tree-inorder-traversal/) | **迭代栈**：一路压左，弹出访问，再转向右子树。 |
| **二叉树** | 98. 验证二叉搜索树 | [Link](https://leetcode.cn/problems/validate-binary-search-tree/) | **中序递增**：中序遍历应严格递增；或递归携带上下界。 |
| **二叉树** | 101. 对称二叉树 | [Link](https://leetcode.cn/problems/symmetric-tree/) | **镜像递归/队列**：比较左左与右右、左右与右左。 |
| **二叉树** | 102. 二叉树的层序遍历 | [Link](https://leetcode.cn/problems/binary-tree-level-order-traversal/) | **BFS**：队列按层推进，记录每层节点数。 |
| **二叉树** | 104. 二叉树的最大深度 | [Link](https://leetcode.cn/problems/maximum-depth-of-binary-tree/) | **DFS/BFS**：递归返回 `1+max(left,right)` 或按层统计。 |
| **二叉树** | 105. 从前序与中序遍历序列构造二叉树 | [Link](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | **递归分治**：前序定根，中序分左右；哈希表加速定位根下标。 |
| **二叉树** | 114. 二叉树展开为链表 | [Link](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/) | **原地调整**：把左子树接到右边，右子树挂到左子树最右节点。 |
| **二叉树** | 124. 二叉树中的最大路径和 | [Link](https://leetcode.cn/problems/binary-tree-maximum-path-sum/) | **树形 DP**：返回“向上贡献”最大值；全局更新“左右+根”。 |
| **二叉树** | 226. 翻转二叉树 | [Link](https://leetcode.cn/problems/invert-binary-tree/) | **递归/迭代**：交换左右子树。 |
| **二叉树** | 236. 二叉树的最近公共祖先 | [Link](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/) | **后序递归**：左右分别找 p/q，二者都非空则当前为 LCA。 |
| **二叉树** | 297. 二叉树的序列化与反序列化 | [Link](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/) | **BFS/DFS 编码**：用 `null` 占位，反序列化按顺序重建。 |
| **二叉树** | 437. 路径总和 III | [Link](https://leetcode.cn/problems/path-sum-iii/) | **前缀和+哈希**：DFS 时统计到当前的前缀和次数，O(n)。 |
| **二叉树** | 538. 把二叉搜索树转换为累加树 | [Link](https://leetcode.cn/problems/convert-bst-to-greater-tree/) | **反中序遍历**：右-根-左累加前缀和，更新节点值。 |
| **二叉树** | 543. 二叉树的直径 | [Link](https://leetcode.cn/problems/diameter-of-binary-tree/) | **DFS**：返回高度；全局更新 `leftHeight+rightHeight`。 |
| **二叉树** | 617. 合并二叉树 | [Link](https://leetcode.cn/problems/merge-two-binary-trees/) | **递归**：两节点都存在则相加，否则返回非空子树。 |

## 图论

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **图论** | 200. 岛屿数量 | [Link](https://leetcode.cn/problems/number-of-islands/) | **DFS/BFS**：遇到陆地就淹没整块连通区域并计数。 |
| **图论** | 994. 腐烂的橘子 | [Link](https://leetcode.cn/problems/rotting-oranges/) | **BFS**: 计数新鲜橘子，BFS结束后 按照是否有剩余新鲜橘子，返回输出；特例：fresh == 0, 直接返回0，不是-1|
| **图论** | 207. 课程表 | [Link](https://leetcode.cn/problems/course-schedule/) | **拓扑排序**：入度+队列；或 DFS 判环。 |
| **图论** | 399. 除法求值 | [Link](https://leetcode.cn/problems/evaluate-division/) | **建图+搜索**：边权为倍数，DFS/BFS 求路径乘积；或并查集带权。 |


## 回溯

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **回溯** | 17. 电话号码的字母组合 | [Link](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/) | **回溯枚举**：按位选字母，递归构造所有组合。 |
| **回溯** | 22. 括号生成 | [Link](https://leetcode.cn/problems/generate-parentheses/) | **回溯剪枝**：左括号可用则加；右括号数 < 左括号数才可加。 |
| **回溯** | 39. 组合总和 | [Link](https://leetcode.cn/problems/combination-sum/) | **回溯**：可重复选同元素，控制起点避免排列重复。 |
| **回溯** | 46. 全排列 | [Link](https://leetcode.cn/problems/permutations/) | **回溯**：`used` 标记选择状态，逐位填充。 |
| **回溯** | 78. 子集 | [Link](https://leetcode.cn/problems/subsets/) | **回溯/迭代**：每个元素选/不选；或逐步扩展结果集。 |
| **回溯** | 79. 单词搜索 | [Link](https://leetcode.cn/problems/word-search/) | **DFS 回溯**：网格四方向搜索，访问标记+回退。 |
| **回溯** | 301. 删除无效的括号 | [Link](https://leetcode.cn/problems/remove-invalid-parentheses/) | **BFS/回溯剪枝**：最少删除；BFS 按层保证最优，去重。 |

## 二分查找

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **二分查找** | 4. 寻找两个正序数组的中位数 | [Link](https://leetcode.cn/problems/median-of-two-sorted-arrays/) | **二分切分**：在较短数组二分切分点，使左半最大 ≤ 右半最小。 |
| **二分查找** | 74. 搜索二维矩阵 | [Link](https://leetcode.cn/problems/search-a-2d-matrix/) | 从"左下角"/"右上角"开始对比 |
| **二分查找** | 33. 搜索旋转排序数组 | [Link](https://leetcode.cn/problems/search-in-rotated-sorted-array/) | **二分**：判断哪半段有序，再决定丢弃另一半。 |
| **二分查找** | 34. 在排序数组中查找元素的第一个和最后一个位置 | [Link](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/) | **二分两次**：分别找 lower_bound 与 upper_bound-1。 |

## 栈

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **栈** | 20. 有效的括号 | [Link](https://leetcode.cn/problems/valid-parentheses/) | **栈匹配**：遇左括号入栈，遇右括号检查栈顶是否匹配。 |
| **栈** | 394. 字符串解码 | [Link](https://leetcode.cn/problems/decode-string/) | **双栈模拟**：遇 `[`，将积累的数字、(位于数字前的)字符串(初始化为"")均压栈；遇 `]`，出栈拼接，此时未压栈的积累的字符串 可被栈顶数字多次重复，再与作为前缀的栈顶字符串拼接|

## 单调栈

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **单调栈** | 84. 柱状图中最大的矩形 | [Link](https://leetcode.cn/problems/largest-rectangle-in-histogram/) | **单调递增栈**：出栈时计算以该高度为最矮的最大宽度。 |
| **单调栈** | 85. 最大矩形 | [Link](https://leetcode.cn/problems/maximal-rectangle/) | **转化为 84**：逐行累积高度，行内用单调栈求最大矩形。 |
| **单调栈** | 739. 每日温度 | [Link](https://leetcode.cn/problems/daily-temperatures/) | **单调递减栈**：栈存索引，遇更高温度出栈计算间隔。 |

## 堆

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **堆** | 215. 数组中的第K个最大元素 | [Link](https://leetcode.cn/problems/kth-largest-element-in-an-array/) | **堆/快速选择**：小根堆保留 K 个；或 quickselect 平均 O(n)。 |
| **堆** | 253. 会议室 II | [Link](https://leetcode.cn/problems/meeting-rooms-ii/) | **最小堆**：按开始时间排序，堆顶是最早结束；能复用就弹出。 |
| **堆** | 347. 前 K 个高频元素 | [Link](https://leetcode.cn/problems/top-k-frequent-elements/) | **哈希+堆**：统计频次，用大小为 K 的小根堆筛选。 |

## 单调队列

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **单调队列** | 239. 滑动窗口最大值 | [Link](https://leetcode.cn/problems/sliding-window-maximum/) | **单调递减队列**：队首为最大值；移出窗口时弹出过期元素。 |

## 贪心

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **贪心** | 55. 跳跃游戏 | [Link](https://leetcode.cn/problems/jump-game/) | **贪心**：维护当前能到的最远位置 `far`，遍历更新。 |
| **贪心** | 121. 买卖股票的最佳时机 | [Link](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/) | **一次遍历**：维护历史最低价，更新最大利润。 |
| **贪心** | 406. 根据身高重建队列 | [Link](https://leetcode.cn/problems/queue-reconstruction-by-height/) | **排序+插入**：身高降序、k 升序，然后按 k 位置插入。 |
| **贪心** | 621. 任务调度器 | [Link](https://leetcode.cn/problems/task-scheduler/) | **计数/公式**：由最高频任务决定骨架，答案为 `max(nTasks, (maxCnt-1)*(n+1)+numMax)`。 |

## 动态规划

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **动态规划** | 10. 正则表达式匹配 | [Link](https://leetcode.cn/problems/regular-expression-matching/) | **DP**：`dp[i][j]` 表示前缀匹配；重点处理 `*` 的 0 次/多次转移。 |
| **动态规划** | 32. 最长有效括号 | [Link](https://leetcode.cn/problems/longest-valid-parentheses/) | **DP/栈**：常用 `dp[i]` 以 i 结尾长度；或栈存索引求区间。 |
| **动态规划** | 53. 最大子数组和 | [Link](https://leetcode.cn/problems/maximum-subarray/) | **Kadane**：`cur=max(nums[i],cur+nums[i])`，维护全局最大。 |
| **动态规划** | 62. 不同路径 | [Link](https://leetcode.cn/problems/unique-paths/) | **DP**：`dp[i][j]=dp[i-1][j]+dp[i][j-1]`。 |
| **动态规划** | 64. 最小路径和 | [Link](https://leetcode.cn/problems/minimum-path-sum/) | **DP**：`dp[i][j]=min(上,左)+grid[i][j]`，可原地滚动。 |
| **动态规划** | 70. 爬楼梯 | [Link](https://leetcode.cn/problems/climbing-stairs/) | **斐波那契**：`dp[i]=dp[i-1]+dp[i-2]`，滚动变量。 |
| **动态规划** | 72. 编辑距离 | [Link](https://leetcode.cn/problems/edit-distance/) | **二维 DP**：插入/删除/替换三种操作取最小，注意边界初始化。 |
| **动态规划** | 96. 不同的二叉搜索树 | [Link](https://leetcode.cn/problems/unique-binary-search-trees/) | **Catalan DP**：`dp[n]=Σ dp[i-1]*dp[n-i]`。 |
| **动态规划** | 139. 单词拆分 | [Link](https://leetcode.cn/problems/word-break/) | **DP**：`dp[i]` 表示前 i 可拆；枚举切分点+字典集合。 |
| **动态规划** | 152. 乘积最大子数组 | [Link](https://leetcode.cn/problems/maximum-product-subarray/) | **DP**：同时维护当前位置最大/最小乘积（负数会翻转）。 |
| **动态规划** | 198. 打家劫舍 | [Link](https://leetcode.cn/problems/house-robber/) | **DP**：`dp[i]=max(dp[i-1],dp[i-2]+nums[i])`。 |
| **动态规划** | 221. 最大正方形 | [Link](https://leetcode.cn/problems/maximal-square/) | **DP**：`dp[i][j]=min(上,左,左上)+1`（当该格为 1）。 |
| **动态规划** | 279. 完全平方数 | [Link](https://leetcode.cn/problems/perfect-squares/) | **完全背包 DP**：`dp[i]=min(dp[i],dp[i-sq]+1)`。 |
| **动态规划** | 300. 最长递增子序列 | [Link](https://leetcode.cn/problems/longest-increasing-subsequence/) | **贪心+二分**：维护 `tails`，对每个数二分替换位置。 |
| **动态规划** | 309. 最佳买卖股票时机含冷冻期 | [Link](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/) | **状态机 DP**：持有/卖出/冷冻（或 rest），按天转移。 |
| **动态规划** | 312. 戳气球 | [Link](https://leetcode.cn/problems/burst-balloons/) | **区间 DP**：枚举最后戳破的气球 k，`dp[l][r]=max(dp[l][k]+dp[k][r]+gain)`。 |
| **动态规划** | 322. 零钱兑换 | [Link](https://leetcode.cn/problems/coin-change/) | **完全背包**：`dp[i]=min(dp[i],dp[i-coin]+1)`，初始化无穷大。 |
| **动态规划** | 337. 打家劫舍 III | [Link](https://leetcode.cn/problems/house-robber-iii/) | **树形 DP**：每节点返回选/不选两种收益。 |
| **动态规划** | 416. 分割等和子集 | [Link](https://leetcode.cn/problems/partition-equal-subset-sum/) | **0/1 背包**：目标为总和一半，`dp[j]=dp[j]||dp[j-num]`。 |
| **动态规划** | 494. 目标和 | [Link](https://leetcode.cn/problems/target-sum/) | **转化背包**：转为求子集和 \( (sum+target)/2 \) 的方案数。 |

## 位运算

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **位运算** | 136. 只出现一次的数字 | [Link](https://leetcode.cn/problems/single-number/) | **异或**：成对抵消，剩下的就是答案。 |
| **位运算** | 338. 比特位计数 | [Link](https://leetcode.cn/problems/counting-bits/) | **DP**：`dp[i]=dp[i>>1]+(i&1)` 或 `dp[i]=dp[i&(i-1)]+1`。 |
| **位运算** | 461. 汉明距离 | [Link](https://leetcode.cn/problems/hamming-distance/) | **异或+计数**：`x^y` 后统计 1 的个数（`x&=x-1`）。 |

## 设计

| 分类 | 题目 | 链接 | 核心解法思路 |
| --- | --- | --- | --- |
| **设计** | 146. LRU 缓存 | [Link](https://leetcode.cn/problems/lru-cache/) | **哈希+双向链表**：O(1) get/put；链表维护最近使用顺序。 |
| **设计** | 155. 最小栈 | [Link](https://leetcode.cn/problems/min-stack/) | **辅助栈**：同步保存当前最小值，实现 O(1) getMin。 |
| **设计** | 208. 实现 Trie (前缀树) | [Link](https://leetcode.cn/problems/implement-trie-prefix-tree/) | **字典树**：节点存子指针与单词结束标记，支持前缀查询。 |
