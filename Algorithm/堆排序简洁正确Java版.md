

```java
public void headSort(int[] nums){
  for(int i=nums.length/2-1;i>=0;--i) heapAdjust(nums,i,nums.length-1);
  for(int i=nums.length-1;i>0;--i){
    swap(nums,0,i);
    heap(nums,0,i-1);
  }
}
```



```java
private static void heapAdjust(int[] nums,int i,int n){
  int index,value;
  index = i,value = nums[index];
  for(int j=i*2+1;j<=n;j=j*2+1){
    if(j<n && nums[j]<nums[j+1]) ++j;
    if(value > nums[j]) break;
    nums[index] = nums[j];
    index = j;
  }
  nums[index] = value;
}
```



```java
private static void swap(int[] nums,int index1,int index2){
  nums[index1] = nums[index1] ^ nums[index2];
  nums[index2] = nums[index1] ^ nums[index2];
  nums[index1] = nums[index1] ^ nums[index2];
}
```

