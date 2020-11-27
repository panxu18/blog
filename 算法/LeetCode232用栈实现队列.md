> 使用栈实现队列的下列操作：
>
> push(x) -- 将一个元素放入队列的尾部。
> pop() -- 从队列首部移除元素。
> peek() -- 返回队列首部的元素。
> empty() -- 返回队列是否为空。
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/implement-queue-using-stacks

```java
class MyQueue {



    Stack<Integer> stack0, stack1;
    /** Initialize your data structure here. */
    public MyQueue() {
        stack0 = new Stack<>();
        stack1 = new Stack<>();
    }

    /** Push element x to the back of queue. */
    public void push(int x) {
        stack1.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if (stack0.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack0.push(stack1.pop());
            }
        }
        return stack0.pop();
    }

    /** Get the front element. */
    public int peek() {
        if (stack0.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack0.push(stack1.pop());
            }
        }
        return stack0.peek();
    }

    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stack0.isEmpty() && stack1.isEmpty();
    }
}
```

