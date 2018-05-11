快速排序

```java
public void quickSort(ListNode begin , ListNode end){
  if(begin==null||begin==end) return;
  ListNode pivot = partition(begin,end);
  quickSort(begin,pivot);
  quickSort(pivot.next,end);
}
```



```java
public ListNode(ListNode begin,ListNode end){
         if(begin == null || begin == end)
             return begin;        
         int val = begin.val;  //基准元素
         ListNode index = begin, cur = begin.next;
         
         while(cur != end){
            if(cur.val < val){  //交换
               index = index.next;
               int tmp = cur.val;
               cur.val = index.val;
               index.val = tmp;
            }
            cur = cur.next;
         }
                 
         begin.val = index.val;
         index.val = val;
         
         return index;
}
```





归并排序

```java
//找到中间点，然后分割
    public ListNode getMiddle(ListNode head) {
        if (head == null) {
            return head;
        }
        //快慢指针
        ListNode slow, fast;
        slow = fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
```



```java
// 合并排好序的链表
    public ListNode merge(ListNode a, ListNode b) {
        ListNode dummyHead, curr;
        dummyHead = new ListNode(0);
        curr = dummyHead;
        while (a != null && b != null) {
            if (a.val <= b.val) {
                curr.next = a;
                a = a.next;
            } else {
                curr.next = b;
                b = b.next;
            }
            curr = curr.next;
        }
        curr.next = (a == null) ? b : a;
        return dummyHead.next;
    }
```



```java
//单链表的归并排序
    public ListNode merge_sort(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        //得到链表中间的数
        ListNode middle = getMiddle(head);
        ListNode sHalf = middle.next;
        //拆分链表
        middle.next = null;
        //递归调用
        return merge(merge_sort(head), merge_sort(sHalf));
    }
```

