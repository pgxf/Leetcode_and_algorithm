```java
public boolean hasTree(TreeNode root,TreeNode node){
  if(root==null||node==null) return false;
  boolean has = false;
  if(equal(root,node)) has = judge(root,node);
  if(!has) has = judge(root.left,node);
  if(!has) has = judge(root.right,node);
  return has;
}
```



```java
public boolean judge(TreeNode root,TreeNode node){
  if(root==null) return false;
  if(node==null) return true;
  if(!equal(root,node)) return false;
  return judge(root.left,node.left)&&judge(root.right,node.right);
}
```

