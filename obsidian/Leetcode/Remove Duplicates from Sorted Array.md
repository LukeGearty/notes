## Problem
Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**. Then return _the number of unique elements in_ `nums`.

Consider the number of unique elements of `nums` to be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the unique elements in the order they were present in `nums` initially. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}

If all assertions pass, then your solution will be **accepted**.

**Example 1:**

**Input:** nums = [1,1,2]
**Output:** 2, nums = [1,2,_]
**Explanation:** Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

**Example 2:**

**Input:** nums = [0,0,1,1,1,2,2,3,3,4]
**Output:** 5, nums = [0,1,2,3,4,_,_,_,_,_]
**Explanation:** Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

**Constraints:**

- `1 <= nums.length <= 3 * 10^4`
- `-100 <= nums[i] <= 100`
- `nums` is sorted in **non-decreasing** order.


## Solution 1
Declare array temp, and int k = 0
1. Iterate through array nums, i to nums.length
2. if nums[i] is not in temp, put it in temp and increment k

Iterate through temp, i to temp.length
1. nums[i] = temp[i]

return k

```C++
int removeDuplicates(vector<int>& nums) {

	int k = 0; // will increase as it encounters unique numbers

	vector<int> temp;

  

	for (int i = 0; i < nums.size(); i++) {

		if (find(temp.begin(), temp.end(), nums[i]) == temp.end()) {

			temp.push_back(nums[i]);

			k++;

		}

	}

	for (int i = 0; i < temp.size(); i++) {

		nums[i] = temp[i];

	}

  

	return k;

}
```

Time Complexity: O(N^2)
Space Complexity: O(N)


## Solution 2


```C++

int removeDuplicates(vector<int>& nums) {

	int k = 1;

	for (int i = 1; i < nums.size(); i++) {

		if (nums[i] != nums[k - 1]) {

			nums[k] = nums[i];

			k++;

		}

	}

	return k;

}
```

### Explanation
Example Input: nums = [1, 1, 2]

int k = 1
for int = 1 to nums.length:

First iteration: i = 1, k = 1
	nums[i] = 1
	nums[k - 1] = 1
	1 == 1
	nums is still = [1, 1, 2]
	move on
Second iteration: i = 2, k = 1
	nums[i] = 2
	nums[k] = 1
	2 != 1
	nums[k] = nums[i]
	nums = [1, 2, \*]
	k++
It stops, return k.
	

Time Complexity: O(N)
Space Complexity: O(1)

