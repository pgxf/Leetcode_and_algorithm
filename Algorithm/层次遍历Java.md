

```java
public void levelTrav(TreeNode root){
  if(root == null) return ;
  TreeNode node = root;
  Queue<TreeNode> queue = new ArrayDeque<>();
  queue.add(node);
  if(!queue.isEmpty()){
    node = queue.peek();
    System.out.println(node.value);
    if(node.left!=null) queue.add(node.left);
    if(node.right!=null) queue.add(node.right);
    queue.poll();
  }
}
```



