**格式优美**





```java
class Solution{
  
  public static void quickSort(int[] arr){
    qsort(arr,0,arr.length-1);
  }
  
  private static void qsort(int[] arr, int low ,int high){
    int pivot = partition(arr ,low , high);
    qsort(arr, low , pivot-1);
    qsort(arr, pivot+1, high);
  }
  
  private static void partition(int[] arr, int low ,int high){
    int pivot = arr[low];
    while(low<high){
      while(low<high && arr[high] >= pivot) --high;
      arr[low] = arr[high];
      while(low<high && arr[low] <= pivot) ++low;
      arr[high] = arr[low];
    }
    arr[low] = pivot;
    return low;
  }
}
```

