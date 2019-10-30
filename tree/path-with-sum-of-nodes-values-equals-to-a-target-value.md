# Path with Sum of Nodes Values Equals to a Target Value

#### 问题：给出一个二叉树和一个目标整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。\(注意: 在返回值的list中，数组长度大的数组靠前\)

例：

![](../.gitbook/assets/image%20%283%29.png)

上面的图能看出，Expected Output为 `[[2, 1, 1]]`。`[4]`并不是其中一个解，因为`[4]`不能形成一个路径。只有从根节点 2 到叶节点 才算一个路径。

```python
def pathSum(root, target):
    if not root:
        return []
    if not root.left and not root.right and root.val == target:
        return [[root.val]]
    res = []
    left = pathSum(root.left, target - root.val)
    right = pathSum(root.right, target - root.val)
    for i in left + right:
        res.append([root.val] + i)
    return res
```

![](../.gitbook/assets/image%20%282%29.png)





















