# Reconstruct Binary Tree from Preorder and Inorder Traversal

## Reconstruct Binary Tree from Preorder and Inorder Traversal

**Question: Return any binary tree that matches the given preorder and inorder traversals. Values in the traversals** `pre` **and** `tin` **\*\*are** distinct positive integers.\*\*

* Example 1:
  * `Input: pre = [1, 2, 4, 7, 3, 5, 6, 8], tin = [4, 7, 2, 1, 5, 3, 8, 6]`
  * `Output: []`
* Note:
  * `1 <= pre.length == tin.length <= 30`
  * `pre[] and tin[]` are both permutations of `1, 2, ..., pre.length`.
  * It is guaranteed an answer exists. If there exists multiple answers, you can return any of them.

**思路：假设我们有下面二叉一棵树：**

![Figure 1. Tree Example](../.gitbook/assets/1572113633-1%20%281%29.jpg)

对于三种遍历方式（Preorder, Inorder, Postorder）来说，它们的顺序分别为：

* Preorder: \[1, 2, 4, 7, 3, 5, 6, 8\]
* Inorder: \[4, 2, 7, 1, 5, 3, 8, 6\]
* Postorder: \[7, 4, 2, 5, 8, 6, 3, 1\]
* `Preorder: root, (left subtree), (right subtree)`
* `Inorder: (left subtree), root, (right subtree)`
* `Postorder: (left subtree), (right subtree), root`

```python
def re(pre, tin):
    if len(pre) <= 0 or len(tin) <= 0:
        return
    root = TreeNode(pre[0])                            # 每一层递归中，当前层的pre[0]一定是当前层子树的根节点
    for i in range(len(tin)):                        # 遍历tin，找到tin[i] == pre[0]。此时 i 就是当前层根节点在 tin 中的索引值
        if tin[i] == pre[0]:
            root.left = re(pre[1:i+1], tin[:i]
            root.right = re(pre[i+1:], tin[i+1:]
            return root
```

```text
解释：

 第一层的root
       |
pre = [1, 2, 4, 7, 3, 5, 6, 8]
tin = [4, 2, 7, 1, 5, 3, 8, 6]
                |
          第一层的root，索引值为 i
所以，tin中，root 之前的（4，2，7）就是root的左子树；root之后的（5，3，8，6）就是root的右子树
同理，pre中，root之后第一个元素（2）到第 i 个元素（7，包含）就是root的左子树；从第 (i+1)个元素（3） 到最后一个元素（8）就是root的右子树

根据上面的insight，我们就能递归的求解问题了。
Base case就是当 当前层的 pre 或 tin 为空，此时我们返回 None
```

