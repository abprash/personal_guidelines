# Core algorithms with various data structures for interviews

* Arrays
* Strings
* Sliding window
* Two pointers
* [Binary Search](https://github.com/abprash/personal_guidelines/blob/master/Core_Algorithms.md#BinarySearch "Binary Search")
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
* Union find

## Arrays
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
## Binary Search
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
  * Time - Given N elements in the search space, O(Log N) - logarithmic time.
  * Space - Constant complexity,
