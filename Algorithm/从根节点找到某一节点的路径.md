1.首先判断这个树有没有这个节点

```c
bool hasNode(TreeNode root,TreeNode node){
  if(root==node) return true;
  bool has = false;
  if(root.left!=NULL) has = hasNode(root.left,node);
  if(!has && root.right!=NULL) has = hasNode(root.right,node);
  return has;
}
```



2.从根节点到某一个节点的路径

```c
bool getPath(TreeNode root,TreeNode node,List<TreeNode> path){
  if(root==node) return true;
  path.add(root);
  bool found = false;
  if(root.left!=NULL) found = getPath(root.left,node,path);
  if(!found && root.right!=NULL) found = getPath(root.right,node,path);
  if(!found) path.remove(path.size()-1);
  retuen found;
}
```



