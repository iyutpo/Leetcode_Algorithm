# Leetcode 51: N-Queens I

Leetcode 51: N-Queens

The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively. Example: Input: 4 Output: \[ \[".Q..", // Solution 1 "...Q", "Q...", "..Q."\],

\["..Q.", // Solution 2 "Q...", "...Q", ".Q.."\] \] Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above. 国际象棋规则：皇后（Queen）可以在n x n的棋盘内 向上、下、左、右、左上、左下、右上、右下八个方向移动 1 ~ （n-1）个格子。

基本思路是，我们先将一个皇后放到棋盘上任意一个格子中，然后将该皇后能攻击到的所有格子都标记出来（如上图）。然后继续放入第二个皇后，以此类推找到所有可行解。我们要知道，棋盘的每一行中有且仅有一个Queen，同样的，每一列中也有且仅有一个Queen。这个insight非常重要，因为对于减少搜索时的计算量很有帮助。 解题步骤： 首先我们先定义一个函数is\_compatible\(\)来判断已经放置的Queen会不会对新来的Queen造成攻击。返回值为True或False。 然后用Backtracking的方法递归求解。 先来看一下is\_compatible\(\)的代码： def is\_compatible\(prev\_cols\_list, curr\_row, curr\_col\): for prev\_row in range\(curr\_row\): if prev\_cols\_list\[prev\_row\] == curr\_col or curr\_row - prev\_row == abs\(prev\_cols\_list\[prev\_row\] - curr\_col\): return False return True

## prev\_cols\_list: 是已经遍历过的 行 中，可行的Queen们的 列的 集合（是一个数组）

## curr\_row: 是新来的Queen的 行 索引（int）

## curr\_col: 是新来的Queen的 列 索引（int）

上面代码比较难懂的地方在于①为什么用for prev\_row in range\(curr\_row\)；②在于if语句：

举个例子来解释一下①，在我们选择了0~2行的Queen的位置后（如上图所示），此时的prev\_cols\_list = \[3, 6, 2\]，curr\_row 为3，for 循环的范围是从0到2。那么prev\_cols\_list\[prev\_row\]就是之前被放置的Queen们的 列索引。使用这个for循环的好处在于：for循环本身就避免了 行冲突，即新来的Queen的行 一定不与 已被放置的Queen的 行 相同。 ②再来解释下if语句，if语句的做因就是为了避免列冲突和对角线冲突。 对于列冲突，我们用代码表示就是：如果prev\_cols\_list\[prev\_row\] == curr\_col，那么就返回False。 对于对角线冲突，我们给出比较数学一点的表达是：对于任意两个Queen1和Queen2，Queen1和Queen2的行坐标之差的绝对值等于Queen1和Queen2的列坐标之差的绝对值。对角线冲突的代码表示是：curr\_row - prev\_row == abs\(prev\_cols\_list\[prev\_row\] - curr\_col\)。这里，因为curr\_row是prev\_row的上界，所以curr\_row一定大于prev\_row，所以等式左边没有加绝对值。

接下来就是Backtracking的部分。 def bt\(answers, positions, row, n\): if row == n: answers.append\(\['.'  _p + 'Q' + '.'_  \(n-1-p\) for p in positions\]\) return answers for col in range\(n\): if is\_compatible\(positions, row, col\): positions.append\(col\) bt\(answers, positions, row+1, n\) positions.pop\(\) return answers

## answers: 结果，2d-array

## positions: 当前递归层中，已经被放置的Queen的 列索引（对应着is\_compatible函数中的prev\_cols\_list参数。dtype: int list）

## row: 当前被搜索的棋盘方格的 行索引

## n: 棋盘大小

当 row==n时，意味着我们已经搜索到棋盘的最后一行，此时我们将其中一个答案append到answers中并return。 我们用for循环遍历列，用递归遍历 行 来寻找不会被 已放置的Queen攻击的Queen的行索引和列索引。每当 当前列 满足 is\_compatible时，我们对row递归。每次将positions的最后pop出来。

我们来看完整的Python代码： class Solution\(object\): def solveNQueens\(self, n\):

```text
    def is_compatible(prev_cols_list, curr_row, curr_col):
        for prev_row in range(curr_row):
            if prev_cols_list[prev_row] == curr_col or curr_row - prev_row == abs(prev_cols_list[prev_row] - curr_col):
                return False
        return True

    def bt(answers, positions, row, n):
        if row == n:
            answers.append(['.' * p + 'Q' + '.' * (n-1-p) for p in positions])
            return answers
        for col in range(n):
            if is_compatible(positions, row, col):
                positions.append(col)
                bt(answers, positions, row+1, n)
                positions.pop()
        return answers

    answers, positions = [], []
    bt(answers, positions, 0, n)
    return answers
```

s = Solution\(\) print\(s.solveNQueens\(4\)\)

> > &gt; \[ \['.Q..', '...Q', 'Q...', '..Q.'\], \['..Q.', 'Q...', '...Q', '.Q..'\] \] Time: O\(n^n\) Space: O\(n\)

