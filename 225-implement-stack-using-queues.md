## [225. Implement Stack using queues](https://leetcode.com/problems/implement-stack-using-queues/)

Implement the following operations of a stack using queues.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
empty() -- Return whether the stack is empty.

Notes:

You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.
Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).

## Solution. Rotate a queue like a circle

```
 _________________
|                 |
<-head, ..., tail<-
```

Loop invariant:

- Before each iteration, elements in the queue are in reversed insertion order.
- Insert new element at the end of the queue and move all preceding elements to the end of the queue.
- After this procedure, the invariant still holds.

1. 1
2. enqueue(2) -> 1, 2

  2.1. dequeue(), enqueue(1) -> 2, 1
3. enqueue(3) -> 2, 1, 3

  3.1. dequeue(), enqueue(2) -> 1, 3, 2
  3.2. dequeue(), enqueue(1) -> 3, 2, 1
...

```java
class MyStack {
	private Queue<Integer> queue = new LinkedList<Integer>();

    // Push element x onto stack.
	public void push(int x) {
		queue.offer(x);
		for (int i = 1; i < queue.size(); i++) {
			queue.offer(queue.poll());
		}
	}

    // Removes the element on top of the stack.
	public void pop() {
		return queue.poll(); 
	}

    // Get the top element.
    public int top() {
		return queue.peek();
    }

    // Return whether the stack is empty.
    public boolean empty() {
       return queue.isEmpty(); 
    }
}
```
