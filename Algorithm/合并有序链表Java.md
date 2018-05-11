```java
public void mergeSort(ListNode pHead1,ListNode pHead2){
  if(pHead1==null) return pHead2;
  if(pHead2==null) return pHead1;
  ListNode pMergeHead = null;
  if(pHead1.value<pHead2.value){
    pMeageHead = pHead1;
    pMergeHead.next = mergeSort(pHead1.next,pHead2);
  }else{
    pMeageHead = pHead2;
    pMergeHead.next = mergeSort(pHead1,pHead2.next);  
  }
  return pMergeHead;
}
```

