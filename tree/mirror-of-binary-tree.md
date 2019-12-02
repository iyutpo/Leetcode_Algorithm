# Mirror of Binary Tree

#### 问题：给出一个二叉树，求该二叉树的镜像

例如我们有如下二叉树，已经镜像后的二叉树：

![](../.gitbook/assets/image%20%2817%29.png)

#### 思路：

这题的思路很简单，我们只要知道了每一层递归需要做的事，就能解出该问题了。

对于每层递归，也就是每一个子树，我们要将：

* \(left subtree\), root, \(right subtree\)  --&gt;  \(right subtree\), root, \(left subtree\)

另外，对于Base Case，如果当前根节点不存在，那就返回None。

代码如下：

```python
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
def mirror(root):
    if not root:
        return
    if root:
        root.left, root.right = root.right, root.left
        if root.left:
            mirror(root.left)
        if root.right:
            mirror(root.right)

# 下面的代码是测试：
r, r.left, r.right = TreeNode(8), TreeNode(6), TreeNode(10)
r.left.left, r.left.right = TreeNode(5), TreeNode(7)
r.right.left = TreeNode(11)
# Define an inorder function to print out node value:
alist = []
def inorder(root):
    if not root:
        return
    inorder(root.left)
    alist.append(root.val)
    inorder(root.right)
mirror(r)        # 树 r 被镜像
inorder(r)        # 中序遍历 镜像后的 r，并将节点值按中序存入 alist
print(alist)

>>> [6,4,4,1,5]
```

下面是图示：



















