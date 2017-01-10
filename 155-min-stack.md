# [155. Min Stack](https://leetcode.com/problems/min-stack/)

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.
Example:
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.

## Solution 1. Two helper stacks

One stack to store elements inserted so far.

Another stack to store the current min for each elements in the stack so far. **Note** the min element changes only if the newly inserted element is smaller than the old min. And to handle the special case when multiple elements are equal to the min, we also push an element to the min stack if it equals the top element of the min stack.

Time: O(1)

Space: O(n)

```java
public class MinStack {
  private final Deque<Integer> nums;
  private final Deque<Integer> mins;

  /** initialize your data structure here. */
  public MinStack() {
    nums = new ArrayDeque<Integer>();
    mins = new ArrayDeque<Integer>();
  }

  public void push(int x) {
    nums.addFirst(x);
    if (mins.isEmpty() || x <= mins.peekFirst()) mins.addFirst(x);
  }

  public void pop() {
    // or use the below statement to compare two integer objects
    // nums.removeFirst().equals(mins.peekFirst())
    if (nums.removeFirst() - mins.peekFirst() == 0) mins.removeFirst();
  }

  public int top() {
    return nums.peekFirst();
  }

  public int getMin() {
    return mins.peekFirst();
  }
}

/**
 * Your MinStack object will be instantiated and called as such:
 *  * MinStack obj = new MinStack();
 *   * obj.push(x);
 *    * obj.pop();
 *     * int param_3 = obj.top();
 *      * int param_4 = obj.getMin();
 *       */
 ```
