# Core algorithms with various data structures for interviews

* Arrays, Strings
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
