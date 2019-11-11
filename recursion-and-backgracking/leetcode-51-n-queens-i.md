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

















