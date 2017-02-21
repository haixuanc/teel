# [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/?tab=Description)

Given a collection of intervals, merge all overlapping intervals.

For example,
Given [1,3],[2,6],[8,10],[15,18],
return [1,6],[8,10],[15,18].

## Solution 1. Sort and merge intervals

Time: O(nlgn)

Space: O(n)

```java
/**
 * Definition for an interval.
 *  * public class Interval {
 *   *     int start;
 *    *     int end;
 *     *     Interval() { start = 0; end = 0; }
 *      *     Interval(int s, int e) { start = s; end = e; }
 */
public class Solution {
  public List<Interval> merge(List<Interval> intervals) {
    if (intervals.size() <= 1) return intervals;
    Collections.sort(intervals, new Comparator<Interval>() {
        @Override
        public int compare(Interval a, Interval b) {
        return a.start == b.start ? 0 : (a.start < b.start ? -1 : 1);
        }
        });
    List<Interval> merged = new ArrayList<Interval>();
    merged.add(intervals.get(0));
    for (int i = 1; i < intervals.size(); i++) {
      if (intervals.get(i).start <= merged.get(merged.size() - 1).end) {
        merged.get(merged.size() - 1).end = Math.max(merged.get(merged.size() - 1).end, intervals.get(i).end);
      } else {
        merged.add(intervals.get(i));
      }
    }
    return merged;
  }
}
```
