```java
public int binSearch(int[] nums,int key){
  if(nums==null||nums.length<=0) return -1;
  int mid,left,right;
  left = 0,right = nums.length-1;
  while(left<=right){
    mid = (right-left)>>1 + left;
    if(key > nums[mid]) left = mid+1;
    else if(key < nums[mid]) right = mid-1;
    else return mid;
}
```





```java
public int binSearch(int[] nums,int key ,int left ,int right){
  if(left > right) return -1;
  mid = (right - left) >>1 + left;
  if(key < nums[mid]) return binSearch(nums,key,left,mid-1);
  else if(key > nums[mid]) return binSearch(nums,key,mid+1,right);
  else return mid;
}
```

