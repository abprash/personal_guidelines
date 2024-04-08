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
* Stacks
* Linked list
* Heaps (or priority queues)
* Tree
* Binary search tree (BST)
* Graphs
* Trie
* Backtracking
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
