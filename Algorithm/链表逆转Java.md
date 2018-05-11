```java
public void reverse(ListNode head){
  if(head==null || head.next==null) return;
  ListNode pReverseHead = null;
  ListNode pNode = pHead;
  ListNode pPre = null;
  while(pNode!=null){
    ListNode pNext = pNode.next;
    if(pNext==null) pReverseHead = pNode;
    pNode.next = pPre;
    pPre = pNode;
    pNode = pNext;
  }
}
```

