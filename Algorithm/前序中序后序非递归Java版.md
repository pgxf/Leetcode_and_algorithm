前序遍历

```Java
public void preOrder(TreeNode root)
{
  LinkedList<TreeNode> stack = new LinkedNode<>();
  TreeNode node = root;
  while(node!=null || !stack.isEmpty())
  {
    if(node!=null)
    {
      System.out.println(node.value);
      stack.push(node);
      node = node.left;
    }
    else
    {
      node = stack.pop();
      node = node.right;
    }
  }
}
```



中序遍历

```Java
public void inOrder(TreeNode root){
  LinkedNode<TreeNode> stack = new LinkedNode<>();
  TreeNode node = root;
  while(node!=null || !stack.isEmpty()){
    if(node!=null){
      stack.push(node);
      node = node.left;
    }else{
      node = stack.pop();
      System.out.println(node.value);
      node = node.right;
    }
  }
}
```



后序遍历

```Java
public void postOrder(TreeNode root){
  Stack<TreeNode> s1 = new Stack();
  Stack<TreeNode> s2 = new Stack();
  TreeNode node = root;
  s1.push(node);
  while(!s1.empty()){
    node = s1.pop();
    s2.push(node);
    if(node.left!=null) s1.push(node.left)
    if(node.right!=null) s1.push(node.right);
  }
  while(!s2.empty()){
    System.out.println(s2.value);
  }
}
```

