# 判断二叉树A是不是二叉树B的子结构

#### 问题：

判断二叉树B是不是二叉树A的子结构

例如：

![](../.gitbook/assets/image%20%286%29.png)

![](../.gitbook/assets/image%20%281%29.png)

#### 思路：

首先应该定义一个函数来比较B中每一个子树是否与A相同。我们要能想到几种可能的情况：

1. （对当前节点的比较）root1.val != root2.val  --&gt; return False   
2. （对左子树的比较，要用递归）return compare\(root1.left, root2.left\)
3. （对右子树的比较，要用递归）return compare\(root1.right, root2.right\)
4. Base Case中又分为两种情况：
   1. 一种是 root1 为空，此时我们认为 root1 是 root2 的子结构
   2. 反之，root2 为空，我们认为 root1 不是 root2 的子结构
5. 上面1，2，3这三条可以合并写到最后的  return 语句中。

所以我们可以这样写：

```python
def compare(root1, root2):
    if not root1:
        return True
    if not root2:
        return False
    return (root1.val == root2.val) and compare(root1.left, root2.left) and compare(root1.right, root2.right)
```

此时我们完成了一棵子树的比较。接下来我们只需要对`A，B`两棵树进行递归比较，**只要`B`的子树中有一棵树与`A`相同，就返回`True`，否则就为`False`**。另外Base Case是：如果A树或B树为空，则返回False。

可以这样写：

```python
def subtree(A, B):
    if not A or not B:
        return False
    return subtree(A, B) or subtree(A, B.left) or subtree(A, B.right)
```

#### 最终的代码为：

```python
def compare(root1, root2):
    if not root1:
        return True
    if not root2:
        return False
    return (root1.val == root2.val) and compare(root1.left, root2.left) and compare(root1.right, root2.right)
    
def is_subtree(A, B):
    if not A or not B:
        return False
    return compare(A, B) or compare(A, B.left) or compare(A, B.right)

A = TreeNode(4)
A.left = TreeNode(5)
A.right = TreeNode(6)
A.left.left, A.right.left = TreeNode(1), TreeNode(4)
B, B.left, B.left.right = TreeNode(6), TreeNode(7), TreeNode(3)
B.right, B.right.left = TreeNode(4), TreeNode(5)
B.right.right, B.right.left.left = TreeNode(6), TreeNode(1)
B.right.right.right, B.right.right.left = TreeNode(4), TreeNode(2)
print(HasSubtree(A, B))

>>> True
```















