# Core algorithms with various data structures for interviews

* [Arrays](#Arrays "Arrays")
* Strings
* [Sliding window](#Sliding-window "Sliding Window")
* [Two pointers](#Two-pointers "Two Pointers")
* [Monotonic stack](#Monotonic-stack "Monotonic Stack")
* [Binary Search](#Binary-search "Binary Search")
* Matrix
* Hash maps
* Interval type problems
* [Stacks](#Stacks)
* Linked list
* Heaps (or priority queues)
* Tree
* Binary search tree (BST)
* [Graphs](#Graphs)
* Trie
* [Backtracking](#Backtracking)
* [Union find](#Union-Find "Union Find")

## [Arrays](#Arrays)
* General programming patterns in array problems.
* Below one is for moving zeros to the end of an array in place
  ```
  class Solution {
    public void moveZeroes(int[] nums) {
        // we need a readPtr and a writePtr
        // readPtr will keep going to the next non Zero num in the array
        // writePtr will slowly be incrementing one by one and we'll be writing the non zero nos. one by one.
        
        int writePtr = 0;
        for (int readPtr = 0; readPtr < nums.length; readPtr++) {
            if (nums[readPtr] != 0) {
                nums[writePtr] = nums[readPtr];
                if (readPtr != writePtr)
                    nums[readPtr] = 0;
                writePtr++;
            }
        }
    }
  }
  ```

* Find the majority element in the array. A majority element is an element which appears at least N/2 times in the array. (N = length of the array)
```
class Solution {
    public int majorityElement(int[] nums) {
        // majority element depends on the fact that, there's at least 1 element occurring at least n/2 times
        // intuition - so we count the occurrences

        int count = 0, countVal = nums[0];
        for (int i=0; i<nums.length; i++) {
            if (countVal == nums[i]) {
                count++;
            } else {
                if (count == 0) {
                    // use current val as new countval and count
                    countVal = nums[i];
                    count = 1; // we're counting this as the first occurrence
                } else {
                    count--;
                }
            }
        }
        return countVal;
    }
}
```
* If we want to find the greatest element to the right of a particular index, we can do it in linear time. Linear memory is optional, in case we need to use it multiple times. We'd need to iterate from the end of the array and keep recording the maximum number so far and record it.
* A similar technique can be leveraged for finding the smallest number to the right of a particular index.
```
class Solution {
    public int[] replaceElements(int[] arr) {
        int maxSoFar = -1;
        int nextElem = -1;
        for (int i=arr.length-1; i>=0; i--) {
            maxSoFar = Math.max(nextElem, maxSoFar);
            nextElem = arr[i]; // record this for the next iteration.
            arr[i] = maxSoFar; // since we're overwriting index i
            
        }
}
```
* Say we want to cluster 2 types of numbers (say odd and even) to certain positions in the array, swapping them in place is an efficient approach.
```
Input: nums = [3,1,2,4]
Output: [2,4,3,1]
Explanation: The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.

public int[] sortArrayByParity(int[] nums) {
        // using extra memory - trivial - 2 pointers, beginning is for even, end is for odd
        // constant memory
        int l = 0, r = nums.length -1;
        // l is for even, r = odd nos.
        while (l < r) {
            if (nums[l] % 2 != 0 && nums[r] %2 == 0) {
                // swap
                swap(nums, l, r);
                r--;
                l++;
            } else if (nums[l] % 2 == 0 && nums[r] %2 != 0) {
                // keep going, the numbers are in the right positions
                l++;
                r--;
            } else if (nums[l] % 2 ==0 && nums[r] %2 == 0) {
                // both point to even
                l++;
            } else if (nums[l] %2 == 1 && nums[r] %2 == 1) {
                // both point to odd
                r--;
            }
        }
        return nums;
    }
```
* Maximum subarray problem - Find maximum subarray when there are both negative and positive numbers in an array.
```
class Solution {
    public int maxSubArray(int[] nums) {
        
        long max = Long.MIN_VALUE, currSum = 0;
        for (int i=0; i<nums.length; i++) {
            // 2 cases to handle - negative and positive num
            // we should add a +ve num to the current sum regardless of sum value so far
            // max sub array shouldn't start at a negative num
            
            if (nums[i] >= 0) {
                // include this
                currSum += nums[i];
            } else {
                // if we're starting - do not include
                if (currSum == 0) continue;
                else {
                    // we're resetting it here
                    currSum = (currSum + nums[i] > 0  ? currSum + nums[i] : 0);
                    
                }
            }
            max = Math.max(currSum, max);
        }
        
        // below is to handle the negative number case
        if (max == Long.MIN_VALUE) {
            return Arrays.stream(nums)
                .max()
                .getAsInt();
        }
        return (int) max;
    }
}
```
* Find number of subarrays equal to sum k
```
class Solution {
    public int subarraySum(int[] nums, int k) {
    if (nums == null || nums.length == 0) return 0;
    // TODO - assert other invariants
    int counter = 0;
    int currSum = 0;
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0,1);
    for (int i=0; i<nums.length; i++) {
        currSum += nums[i];
        // currSum is sum until i'th index
        if (map.containsKey(currSum-k))
            counter += map.get(currSum-k);
        
        // put the accum. sum so far
        map.put(currSum, map.getOrDefault(currSum, 0) + 1);
    }
    return counter;
    }
}
```
* Partition labels
```
class Solution {
    public List<Integer> partitionLabels(String s) {
        Map<Character, Integer> map = new HashMap<>();

        for (int i=0; i<s.length(); i++) {
            char c = s.charAt(i);
            map.put(c, i); // this will always have the last occurrence of all letters
        }

        int p1 = 0, p2 = 0;
        int addedParitionLength = 0;
        List<Integer> ans = new ArrayList<>();

        while (p1 < s.length() && p2 < s.length()) {
            char c = s.charAt(p1);
            // set p2 at last index of c, only if it is larger
            p2 = Math.max(p2, map.get(c));
            // when p1 and p2 meet, we know the partition has ended, add it
            if (p1 == p2) {
                if (ans.isEmpty())
                    ans.add(p1 + 1); // since 0 indexed
                else
                    ans.add(p1 - addedParitionLength + 1);
                addedParitionLength += ans.get(ans.size()-1);
            }
            p1++;
        }
        return ans;
    }
}
```
* Prefix sum method -- Can be used to solve range sum type problems.

## [Sliding window](#Sliding-window)
* The basic premise for sliding window problems is to use two pointers.
* For sliding window problems, there are a few basic patterns
  * A window of fixed width
    * This would be used to solve moving average type problems.  
  * Two pointers travelling together (either in fixed or varying speeds)
    * The `fast` and `slow` pointer style is used to detect cycles in linked lists, performing in place array updates.   
  * Two pointers in opposite directions
    * This can be used to group 

## [Two pointers](#Two-pointers)

## [Monotonic stack](#Monotonic-stack)
* Monotonic stacks can be used to find out next immediately greater element or next smaller element for all elements in an array in linear time. (Careful, not to be confused with the greatest/lowest element to the right of a given index).
```
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        // Brute force - straightforward - quadratic time complexity
        // Improved solution - using monotonic stack.
        // variation of using next greater element
        // How ?

        Deque<Integer> deque = new ArrayDeque<>(); // only if a day is cooler than the one on top of stack, we will push it
        int[] ans = new int[temperatures.length];
        for (int i=0; i<temperatures.length; i++) {
            int temp = temperatures[i];
            // if we come across a colder temp -- add it to stack
            // we've come across a warmer temperature - so step back to the indices so far on the stack and pop them
            while (!deque.isEmpty() && temp > temperatures[deque.peekFirst()]) {
                int pastIndex = deque.removeFirst();
                ans[pastIndex] = i - pastIndex;
            }
            deque.addFirst(i); // add current temp as well
        }
        return ans;
    }
}
```
* Variation of the above problem, where we have to find the next smaller or equal element.
```
    public int[] finalPrices(int[] prices) {
        // this is a variation of next greater element.
        // instead of next greater element, we're finding out the next smaller or equal element
        Deque<Integer> deque = new ArrayDeque<>();
        int[] ans = new int[prices.length];
        for (int i=0; i<prices.length; i++) {
            
            int currPrice = prices[i];
            // if curr num. is larger than the past num in the stack, we will push it
            // if curr num, is smaller, we will start processing it, and applying the discount with the nos. in the stack
            while(!deque.isEmpty() && currPrice <= prices[deque.peekFirst()]) {
                int prevIndex = deque.removeFirst();
                ans[prevIndex] = prices[prevIndex] - currPrice; // since currPrice is smaller 
            }
            deque.addFirst(i);
        }
        // the deque will have elements still remaining, so add them as is, if we cannot find a smaller or equal price after it,
        while(!deque.isEmpty()) {
            int popped = deque.removeFirst();
            ans[popped] = prices[popped];
        }
        return ans;
    }
```
## [Binary Search](#Binary-search)
* Typically used for searching a sorted collection in logarithmic time ie O(log N) where N is the number of elements in given collection.
* There could be slight variations of this algorithm implemented based on the type of collection (like, rotated sorted arrays, arrays with duplicates etc.)

General algorithm is like this
```

public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[mid] < target) {
                // target is greater - consider 2nd half
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return -1; <------------- index low also can be returned here since by this time it went past index high, but we can be explicit in case we are searching for something.
    }

```
* Complexity
  * Time - Given N elements in the search space, O(Log N) - logarithmic time, since we halve the search space in each iteration. (log to base 2).
  * Space - Constant space complexity for the above one.
* Other types of problems include
  * Searching in rotated sorted array (without duplicates)
    * The core idea is similar where we use binary search, but since the array is rotated, we want to ensure whenever we check the bounds, we're checking in the bounds which is sorted.
    * If the target is present in the bounds, and the bounds are sorted, we can pick that side, or pick the other one.
  ```
    class Solution {
      public int search(int[] nums, int target) {
          // should identify which side is sorted and decide accordingly
  
          int low = 0, high = nums.length - 1;
          while (low <= high) {
              int mid = low + (high - low) / 2;
              if (nums[mid] == target)
                  return mid;
              if (nums[low] <= nums[mid]) {
                  // left side is sorted
                  if (target <= nums[mid] && target >= nums[low])
                      high = mid - 1;
                  else
                      low = mid + 1;
              } else {
                  if (nums[mid] < target && target <= nums[high])
                      low = mid + 1;
                  else
                      high = mid - 1;
              }
          }
          return -1; // we haven't found the target
      }
  }
  ```
  * Searching in rotated sorted array (with duplicates)
  ```
  ```
  * Find the first and last occurrence of a number in an array with duplicates
  ```
  ```
  * Find minimum in a rotated sorted array
  ```
  class Solution {
      public int findMin(int[] nums) {
          // we need to move towards the pivot pt/ point where it is broken, as that's the starting point for the unrotated array
  
          int low = 0, high = nums.length -1;
          while (low <= high) {
              int mid = low + (high - low)/2;
              if (nums[low] < nums[mid] && nums[mid] < nums[high]) {
                  // we're at a point where it's perfectly sorted. return low
                  return nums[low];
              }
              if (mid - 1 >=0 && nums[mid] < nums[mid -1] && mid+1 < nums.length && nums[mid] < nums[mid + 1]) return nums[mid]; // this is the pivot
              if (nums[mid] > nums[high]) {
                  // pivot is within this range - right half
                  low = mid + 1;
              } else {
                  high = mid - 1;
              }
          }
          return nums[low];
      }
  }
  ```
## [Stacks](#Stacks)
* Stack is a LIFO data structure which is versatile for evaluating expressions, monotonic stack is used to find interesting properties in an array like, finding next immediate greater or lesser element.
* Below is an example of a problem which is used to parse a numerical expression which doesn't contain any parentheses.
```

Input: s = " 3+5 / 2 "
Output: 5


class Solution {
    public int calculate(String s) {
        // core idea is, if operator is + or - do it later, so we store it on top of stack
        // if operator is * or /, do it right away
        Deque<Integer> deque = new ArrayDeque<>();
        int currentNum = 0;
        char operator = '+';

        for (int i = 0; i < s.length();) {
            char c = s.charAt(i);
            if (Character.isWhitespace(c)) {
                i++;
                continue;
            }
            if (Character.isDigit(c)) {
                // keep going until the entire number is parsed
                // set current number - num
                int iter = i;
                int num = 0;
                while (iter < s.length() && Character.isDigit(s.charAt(iter))) {
                    num = num * 10 + Character.getNumericValue(s.charAt(iter));
                    iter++;
                }
                i = iter;
                // check previous operator value
                // since operators are in the middle, the number we parsed could be on the RHS of a previous number.
                if (operator == '-' || operator == '+') {
                    int res = operator == '-' ? -num : num;
                    deque.addFirst(res);
                } else if (operator == '/' || operator == '*') { 
                    // if operator is * or /
                    // we need to evaluate it
                    int res = operator == '*' ? deque.removeFirst() * num : deque.removeFirst() / num;
                    deque.addFirst(res);
                }
            } else {
                // push curr num to the stack
                operator = c;
                i++;
            }
        }
        int res = 0;
        while (!deque.isEmpty()) {
            res += deque.removeFirst();
        }
        return res;
    }
}
```
* Extending the above implementation, we can solve it for expressions containing parentheses.
```
Input: s = "6-4/2"
Output: 4

Input: s = "2*(5+5*2)/3+(6/2+8)"
Output: 21

---

class Solution {
    public int calculate(String s) {
        Deque<Integer> deque = new ArrayDeque<>();
        char operator = '+';
        // System.out.println("incoming string = "+s);
        for (int i = 0; i < s.length();) {
            char c = s.charAt(i);
            if (Character.isWhitespace(c)) {
                i++;
                continue;
            }
            if (Character.isDigit(c)) {
                // keep going until the entire number is parsed
                // set current number - num
                int iter = i;
                int num = 0;
                while (iter < s.length() && Character.isDigit(s.charAt(iter))) {
                    num = num * 10 + Character.getNumericValue(s.charAt(iter));
                    iter++;
                }
                i = iter;
                // check previous operator value
                if (operator == '-' || operator == '+') {
                    int res = operator == '-' ? -num : num;
                    deque.addFirst(res);
                } else if (operator == '/' || operator == '*') { 
                    // if operator is * or /
                    // do the operation and store it back on the stack
                    int res = operator == '*' ? deque.removeFirst() * num : deque.removeFirst() / num;
                    deque.addFirst(res);
                }
            } else if (c == '(') {
                // process this part of the equation recursively
                int count = 0;
                int start = i;
                while (true) {
                    char curr = s.charAt(i);
                    if (curr == '(') count++;
                    else if (curr == ')') count--;
                    if (count == 0) break;
                    i++;
                }
                int num = calculate(s.substring(start+1, i)); // lose the parens
                // check previous operator value
                if (operator == '-' || operator == '+') {
                    int res = operator == '-' ? -num : num;
                    deque.addFirst(res);
                } else if (operator == '/' || operator == '*') { // if operator is * or /
                    int res = operator == '*' ? deque.removeFirst() * num : deque.removeFirst() / num;
                    deque.addFirst(res);
                }
            } else {
                operator = c;
                i++;
            }
        }
        int res = 0;
        while (!deque.isEmpty()) {
            res += deque.removeFirst();
        }
        return res;
    }
}
```
## [Graphs](#Graphs)
* General algorithms for graph includes
  * depth first search - for exploring the whole graph, problems like finding if a connection/path exits, general graph exploration,
  * breadth first search - For finding shortest path **IFF the edges are unweighted**.
  * Topological sorting - Can be used to detect any cycles in the graph, dependency ordering.
  * Minimum spanning tree - Used to find the shortest path connecting all vertices in the graph.
* Topological sorting
```
Topological sorting -- construct indegree map and prune edges 

class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer> ans = new ArrayList<>();

        // build graph
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int [] edge : prerequisites) {
            // [to, from]
            int from = edge[1];
            int to = edge[0];
            if (!graph.containsKey(from))
                graph.put(from, new ArrayList<>());
            if (!graph.containsKey(to))
                graph.put(to, new ArrayList<>());
            graph.get(from).add(to);
        }

        // build indegree map
        Map<Integer, Set<Integer>> indegreeMap = new HashMap<>();
        for(Map.Entry<Integer, List<Integer>> entry : graph.entrySet()) {
            List<Integer> values = entry.getValue();
            int from = entry.getKey();
            for (Integer v : values) {
                if (!indegreeMap.containsKey(v))
                    indegreeMap.put(v, new HashSet<>());
                indegreeMap.get(v).add(from);
            }
        }
        for (int i=0; i<numCourses; i++) {
            if (!indegreeMap.containsKey(i))
                indegreeMap.put(i, new HashSet<>()); // need this to get starting point
        }

        // start topo sorting
        Deque<Integer> deque = new ArrayDeque<>();
        Integer start = getZeroIndegreeNode(indegreeMap);
        if (start == null)
            return new int[]{};
        deque.addFirst(start);

        while(!deque.isEmpty()) {
            int curr = deque.removeFirst();
            // add this one
            ans.add(curr);
            // get neighbors
            List<Integer> neighbors = graph.getOrDefault(curr, new ArrayList<>());
            for (int n : neighbors) {
                // remove the indegree edge
                indegreeMap.get(n).remove(curr);
            }
            // add back nodes to queue with zero indegree
            Integer next = getZeroIndegreeNode(indegreeMap);
            if (next != null)
                deque.addLast(next);
        }
        // System.out.println(ans);
        int[] res = new int[ans.size()];
        for (int i=0; i<ans.size(); i++)
            res[i] = ans.get(i);
        // check if computed result is valid
        return res.length == numCourses ? res : new int[]{};
    }

    private Integer getZeroIndegreeNode(Map<Integer, Set<Integer>> indegreeMap) {
        Integer ans = indegreeMap.entrySet()
            .stream()
            .filter(entry -> entry.getValue().size() == 0)
            .map(entry -> entry.getKey())
            .findFirst().orElse(null);
        if (ans != null && indegreeMap.get(ans).size() == 0) //delete it
            indegreeMap.remove(ans);
        return ans; 
    }

}
```
* Minimum spanning tree -- We use Prim's algorithm to find out the minimum spanning tree.
```
Complexity - O(N^2 logN)
Space - O(N) linear
---
    public static int manhattan_distance(int[] p1, int[] p2) {
        return Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1]);
    }

    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        boolean[] visited = new boolean[n];
        HashMap<Integer, Integer> heap_dict = new HashMap<>();
        heap_dict.put(0, 0);
        
        PriorityQueue<int[]> min_heap = new PriorityQueue<>((a, b) -> Integer.compare(a[0], b[0]));
        min_heap.add(new int[]{0, 0});
        
        int mst_weight = 0;
        
        while (!min_heap.isEmpty()) {
            int[] top = min_heap.poll();
            int w = top[0], u = top[1];
            
            if (visited[u] || heap_dict.getOrDefault(u, Integer.MAX_VALUE) < w) continue;
            
            visited[u] = true;
            mst_weight += w;
            
            for (int v = 0; v < n; ++v) {
                if (!visited[v]) {
                    int new_distance = manhattan_distance(points[u], points[v]);
                    if (new_distance < heap_dict.getOrDefault(v, Integer.MAX_VALUE)) {
                        heap_dict.put(v, new_distance);
                        min_heap.add(new int[]{new_distance, v});
                    }
                }
            }
        }
        
        return mst_weight;
    }
```
* Single Shortest path - Dijkstra
```
There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.

You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

---

class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        Map<Integer, List<int[]>> adj = new HashMap<>();
        for (int[] i : flights) // E
            adj.computeIfAbsent(i[0], value -> new ArrayList<>()).add(new int[] { i[1], i[2] });

        int[] stops = new int[n]; // V
        Arrays.fill(stops, Integer.MAX_VALUE);
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        // {dist_from_src_node, node, number_of_stops_from_src_node}
        pq.offer(new int[] { 0, src, 0 });

        while (!pq.isEmpty()) {
            int[] temp = pq.poll(); // O(1)
            int dist = temp[0];
            int node = temp[1];
            int steps = temp[2];
            // We have already encountered a path with a lower cost and fewer stops,
            // or the number of stops exceeds the limit.
            if (steps > stops[node] || steps > k + 1)
                continue;
            stops[node] = steps;
            if (node == dst)
                return dist;
            if (!adj.containsKey(node))
                continue;
            for (int[] a : adj.get(node)) { // V
                pq.offer(new int[] { dist + a[1], a[0], steps + 1 }); // log E 
            }
        }
        return -1;
    }
}
```
## [Backtracking](#Backtracking)
* Backtracking is a brute force approach of trying out all possible solutions and then checking if each solution is a fit.
* Typical types of problems include, sudoku solvers, N-Queens problem, combinations, permutations, subset problems.
* Permutation 1 - Get the permutation of given array - provided all numbers are distinct.
```
/*
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
*/

Time complexity -- Time complexity, what you should say in an interview: O(n * n!) -- an approximate growth can be modeled by substituting n with length and finding the length of result.

class Solution {
    List<List<Integer>> ans;
    public List<List<Integer>> permute(int[] nums) {
        ans = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        permutationHelper(nums, 0, new ArrayList<>(), visited);
        return ans;
    }

    private void permutationHelper(int[] nums, int start, List<Integer> currList, boolean[] visited) {
        int len = nums.length;
        if (currList.size() == len) {
            ans.add(new ArrayList(currList));
            return;
        }
        for (int i=0; i<len; i++) {
            int index = (i + start) % len;
            if (visited[index]) continue;
            currList.add(nums[index]);
            visited[index] = true;
            permutationHelper(nums, index + 1, currList, visited);
            currList.remove(currList.size()-1);
            visited[index] = false;
        }
    }
}
```
* Permutation 2 - Get the permutation of given array - array has duplicates.
```

Time complexity, what you should say in an interview: O(n * n! + N log N) (where n is the number of distinct elements, N is the total length)


class Solution {
    List<List<Integer>> ans;

    public List<List<Integer>> permuteUnique(int[] nums) {
        ans = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        Arrays.sort(nums); // this is important to handle duplicates
        permutationHelper(nums, 0, new ArrayList<>(), visited);
        return ans;
    }

    private void permutationHelper(int[] nums, int start, List<Integer> currList, boolean[] visited) {
        int len = nums.length;
        if (currList.size() == len) {
            ans.add(new ArrayList(currList));
            return;
        }
        for (int i=0; i<len; i++) {
            int index = (i + start) % len;
            if (visited[index]) continue; // ensure we don't visit the same number again
            // to handle duplicates - do not visit a previously visited index when the number is equal to the neighbor
            if (index > 0 && nums[index-1] == nums[index] && visited[index-1]) { 
                continue;
            }
            currList.add(nums[index]);
            visited[index] = true;
            permutationHelper(nums, index + 1, currList, visited);
            currList.remove(currList.size()-1);
            visited[index] = false;
        }
    }
}
```
## [Union Find](#Union-Find)
* This is a first iteration of the union find algorithm with a quick find method. This is an eager evaluation.
  ```
  time complexities
  ---
  initialization = O(N) linear
  union = O(N) linear
  find = O(1) constant
  ```
Code:
```
public final class UF1 {

    private final int[] components;

    // construction + initialization of the components array
    public UF1(int n) {
        components = new int[n];
        for (int i=0; i<n; i++) {
            components[i] = i;
        }
    }

    public void union(int p, int q) {
        int rootP = components[p];
        int rootQ = components[q];
        if (rootP != rootQ) {
            for (int i=0; i<components.length; i++) {
                if (components[i] == rootP) {
                    components[i] = rootQ;
                }
            }
        }
    }

    // this is the find method
    public boolean isConnected(int p, int q) {
        return components[p] == components[q];
    }
}
```
* Approach 2 - Quick union. Lazy approach.
```
public class UF2 {

    private final int[] components;

    // construction + initialization of the components array
    public UF2(int n) {
        components = IntStream.rangeClosed(0, n).toArray();
    }

    public void union(int p, int q) {
        int rootP = root(p);
        int rootQ = root(q);
        // now that we've found the roots of both, we can make either one point to the other
        components[rootP] = rootQ;
    }

    public boolean isConnected(int p, int q) {
        return root(p) == root(q);
    }

    // helper method to find the root of a given component
    private int root(int a) {
        // chase parent pointers until we reach the root
        while(components[a] != a) {
            a = components[a];
        }
        return a;
    }
}
```
* Approach 3 - Improving the previous implementation. Weighted quick union.
```
/**
 * Weighted quick union algorithm
 * Improvement to the previous iteration.
 * Basic crux - we'll be considering the weight of the trees when performing union.
 * Always ensuring the smaller tree will become the child of the larger tree (therefore
 *  ensuring the trees don't get too tall)
 *
 *  We accomplish this by having another size array for each node in our components
 *
 *  Run time analysis
 *  ---
 *  Initialization - O(N)
 *  Union - O(log N) (base 2)
 *  find - O(log N) (base 2)
 *
 *
 */
public class UF3 {

    private final int[] components;
    private final int[] sizes;

    // construction + initialization of the components array
    public UF3(int n) {
        sizes = new int[n];
        Arrays.fill(sizes, 1); // initially all of them are disconnected so they're just 1

        components = IntStream.rangeClosed(0, n).toArray();
    }

    public boolean isConnected(int p, int q) {
        return root(p) == root(q);
    }

    public void union(int p, int q) {
        int rootP = root(p);
        int rootQ = root(q);
        if (rootP != rootQ) {
            /**
             * this is where we do the union by rank or weight
             */
            if (sizes[rootP] < sizes[rootQ]) {
                // smaller tree becomes the child of the bigger one. So, we keep some sort of balance
                // add the sizes to the bigger tree
                sizes[rootQ] += sizes[rootP];
                components[rootP] = rootQ; // root of P now points to Q
            } else {
                // do the reverse of the above block
                sizes[rootP] += sizes[rootQ];
                components[rootQ] = rootP;
            }
        }
    }

    private int root(int a) {
        while(a != components[a]) {
            a = components[a];
        }
        return a;
    }
}
```
* Approach 4 - Weighted union find with path compression
```
/**
 * Weighted quick union algorithm with path compression
 * ---
 * Improvement to the previous iteration.
 * In addition to the previous UF with weighted unions, we will incorporate path compression,
 * thereby shortening the path of each node to it's root shorter.
 *
 *  Run time analysis
 *  ---
 *  Initialization - O(N)
 *  Union - O(log N) (M union find operations on a set of N nodes)
 *  find - O(log N) (base 2)
 *
 *
 */
public class UF4 {

    private final int[] components;
    private final int[] sizes;

    // construction + initialization of the components array
    public UF4(int n) {
        sizes = new int[n];
        Arrays.fill(sizes, 1); // initially all of them are disconnected, so they're just 1

        components = IntStream.rangeClosed(0, n).toArray();
    }

    public boolean isConnected(int p, int q) {
        return root(p) == root(q);
    }

    public void union(int p, int q) {
        int rootP = root(p);
        int rootQ = root(q);
        if (rootP != rootQ) {
            /**
             * this is where we do the union by rank or weight
             */
            if (sizes[rootP] < sizes[rootQ]) {
                // smaller tree becomes the child of the bigger one. So, we keep some sort of balance
                // add the sizes to the bigger tree
                sizes[rootQ] += sizes[rootP];
                components[rootP] = rootQ; // root of P now points to Q
            } else {
                // do the reverse of the above block
                sizes[rootP] += sizes[rootQ];
                components[rootQ] = rootP;
            }
        }
    }

    private int root(int a) {
        while(a != components[a]) {
            components[a] = components[components[a]]; /** one pass improvement for path compression.
             Make each node in path, point to its grandparent. Thereby halving path length */
            a = components[a];
        }
        return a;
    }
}
```
