```Java
public void bubbleSort(int[] nums){
  boolean change;
  do{
    change = false;
    for(int i=0;i<nums.length-1;++i){
      if(nums[i]>nums[i+1]){
        nums[i] = nums[i] ^ nums[i+1];
        nums[i+1] = nums[i] ^ nums[i+1];
        nums[i] = nums[i] ^ nums[i+1];
        change = true;
      }
    }
  }while(change);
}
```

