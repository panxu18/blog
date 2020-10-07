> 给你一个链表数组，每个链表都已经按升序排列。
>
> 请你将所有链表合并到一个升序链表中，返回合并后的链表。
>
> 来源：力扣（LeetCode）
>
> 链接： https://leetcode-cn.com/problems/merge-k-sorted-lists/

```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
        return null;
    }
    PriorityQueue<ListNode> minQue = new PriorityQueue<>(Comparator.comparingInt(o->o.val));
    for (int i = 0; i < lists.length; i++) {
        if (lists[i] != null){
            minQue.add(lists[i]);
        }
    }
    ListNode head = new ListNode(-1, null);
    ListNode tail = head;
    while (!minQue.isEmpty()) {
        ListNode min = minQue.poll();
        tail.next = min;
        tail = min;
        min = min.next;
        if (min != null) {
            minQue.offer(min);
        }
    }
    return head.next;
}
```

