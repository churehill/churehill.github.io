---
id: 320
title: 重建二叉树
date: 2015-09-21T18:19:58+00:00
author: tpircsboy
layout: post
guid: http://blog.tpircsboy.com/?p=320
permalink: /acm/reconstructbinarytree/
duoshuo_thread_id:
  - 1340357450817077281
categories:
  - ACM
---
重建二叉树是一个经典问题，由先序遍历序列和中序遍历序列重建二叉树，由后序遍历序列和中序遍历序列重建二叉树。

这个问题的解决方法有两种，一种是递归解决，比较直观，另一种是用stack来做，如下：

```cpp
// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};


class Solution {
    
public:
    // Construct Binary Tree from Preorder and Inorder Traversal 
    // 由先序和中序重建二叉树
    TreeNode* buildTreeInPre(vector&lt;int&gt;& preorder, vector&lt;int&gt;& inorder) {
            if (preorder.empty())
                return NULL;
            stack&lt;TreeNode *&gt; st;
            int pt = 0, it = 0;
            bool flag = false;
            TreeNode *pnode = new TreeNode(preorder[pt++]), *root = pnode;
            st.push(pnode);
            while (pt &lt; preorder.size() || it &lt; inorder.size()) {
                if (!st.empty() && st.top()-&gt;val == inorder[it]) {
                    pnode = st.top(); st.pop(); 
                    it ++;
                    flag = true;
                }
                else {
                    TreeNode* cur = new TreeNode(preorder[pt++]);
                    if (flag) {
                       pnode-&gt;right = cur;
                       flag = false;
                    }
                    else {
                        st.top()-&gt;left = cur;
                    }
                    st.push(cur);
                }
            }
            return root; 
        }

    // Construct Binary Tree from Inorder and Postorder Traversal
    // 由后序和中序重建二叉树
    TreeNode* buildTreeInPost(vector&lt;int&gt;& inorder, vector&lt;int&gt;& postorder) {
        if (inorder.empty())
            return NULL;
        
        stack&lt;TreeNode *&gt; st;
        int pt = postorder.size() - 1, it = pt;
        TreeNode *pnode = new TreeNode(postorder[pt--]), *root = pnode;
        st.push(pnode);
        bool flag = false;
        while (pt &gt;= 0 || it &gt;= 0) {
            if (!st.empty() && st.top()-&gt;val == inorder[it]) {
                pnode = st.top(); st.pop();
                flag = true;
                it --; 
            } 
            else {
                TreeNode* cur = new TreeNode(postorder[pt--]);
                if(flag) {
                    pnode-&gt;left = cur;
                    flag = false; 
                } 
                else {
                    st.top()-&gt;right = cur;
                }
                st.push(cur);
            }
        }
        return root;
    }
};
```

&nbsp;

那么，前序遍历序列和后序遍历序列能否重建出唯一的二叉树？

> 答案是否，前序遍历序列和后序遍历序列所对应的二叉树并不是唯一的。

&nbsp;