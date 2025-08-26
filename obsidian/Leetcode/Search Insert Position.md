## Problem
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [1,3,5,6], target = 5
**Output:** 2

**Example 2:**

**Input:** nums = [1,3,5,6], target = 2
**Output:** 1

**Example 3:**

**Input:** nums = [1,3,5,6], target = 7
**Output:** 4

**Constraints:**

- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` contains **distinct** values sorted in **ascending** order.
- `-10^4 <= target <= 10^4`

## Solution

A simple binary search algorithm, but instead of returning -1 if it isn't found, return whatever value low ends up becoming.

int low = 0
int high = nums.length - 1
int mid

if target > nums[high] 
	return high + 1 

while low <= high
	mid = (low + high) / 2
	if nums[mid] == target
		return mid 
	else if target > nums[mid]
		low = mid + 1
	else
		high = mid - 1

return low

```C++
class Solution {

	public:

		int searchInsert(vector<int>& nums, int target) {

  

			int low = 0;

			int high = nums.size() - 1;
			
			int mid;

  

			if (target > nums[high]) {
			
			return high + 1;
			
			}

  
  

	while (low <= high) {

		mid = (low + high) / 2;

		if (nums[mid] == target) {
			return mid;
		}

		else if (nums[mid] < target) {
			low = mid + 1;

		}
		else {
			high = mid - 1;

		}

	}

  return low;

  }

};
```

Time Complexity: O(log n)
Space Complexity: O(1)