# Leetcode 51: N-Queens I

#### Leetcode 51: N-Queens



The _n_-queens puzzle is the problem of placing _n_ queens on an _n_×_n_ chessboard such that no two queens attack each other.

![](https://assets.leetcode.com/uploads/2018/10/12/8-queens.png)

Given an integer _n_, return all distinct solutions to the _n_-queens puzzle.

Each solution contains a distinct board configuration of the _n_-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space respectively.

**Example:**

```text
Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
```

国际象棋规则：皇后（Queen）可以在n x n的棋盘内 向上、下、左、右、左上、左下、右上、右下八个方向移动 1 ~ （n-1）个格子。

/home/yinghai/Desktop/Screenshot from 2019-11-11 09-57-48.png

![](../.gitbook/assets/screenshot-from-2019-11-11-09-57-48.png)

**基本思路是，我们先将一个皇后放到棋盘上任意一个格子中，然后将该皇后能攻击到的所有格子都标记出来（如上图）。然后继续放入第二个皇后，以此类推找到所有可行解。**

所以我们可以将一个皇后的攻击路线分为直线（上下左右）和斜线（左上、左下、右上、右下）。

斜线又可以表示为：

1. ​左上、右下，用蓝色线条标记
2. 右上、左下，用红色线条标记

如下图，我们能看出，两种不同的斜线各自分别有15条。

![](../.gitbook/assets/image%20%288%29.png)

如果我们用x, y分别表示横纵坐标（左上角是\(0, 0\)），对于红色的攻击路径，我们有

[https://ubuntuforums.org/showthread.php?t=2376213](https://ubuntuforums.org/showthread.php?t=2376213)





























