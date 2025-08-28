## Problem

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

**Input:** nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
**Output:** [1,2,2,3,5,6]
**Explanation:** The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

**Example 2:**

**Input:** nums1 = [1], m = 1, nums2 = [], n = 0
**Output:** [1]
**Explanation:** The arrays we are merging are [1] and [].
The result of the merge is [1].

**Example 3:**

**Input:** nums1 = [0], m = 0, nums2 = [1], n = 1
**Output:** [1]
**Explanation:** The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.

**Constraints:**

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`
- `1 <= m + n <= 200`
- `-109 <= nums1[i], nums2[j] <= 109`

**Follow up:** Can you come up with an algorithm that runs in `O(m + n)` time?


## Solution 1 - Cheating
Set the 0s in num1 to the elements in num2, and then use the sort function

```C++
class Solution {

	public:
	
		void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
			for (int j = 0, i = m; j < n; j++) {
				nums1[i] = nums2[j];
				i++;
			}
			sort(nums1.begin(), nums1.end());
		}
};
```

## Solution 2 - Not Cheating

if n == 0: return
int l = nums1.length

while n > 0 and m > 0:
	if nums2[n - 1] >= nums1[m - 1] :
		nums1[l] = nums2[n - 1]
		n--
	else
		nums1[l] = nums1[m - 1]
		m--
	l--

while n > 0:
	nums1[l] = nums2[n - 1]
	n--
	l--



```C++

class Solution {

	public:
	
	void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
	
	  
	
	if (n == 0) {
	
	return; //nums2 is empty, no sorting necessary
	
	}
	
	  
	
	int l = nums1.size() - 1;
	
	  
	
	while (n > 0 && m > 0) {
	
		if (nums2[n - 1] >= nums1[m -1]) {
	
			nums1[l] = nums2[n - 1];
	
			n--;
	
		} else {
	
			nums1[l] = nums1[m - 1];
	
			m--;
	
		}
	
		l--;
	
	}
	
	
	while (n > 0) {
	
			nums1[l] = nums2[n - 1];
	
			n--;
	
			l--;
	
		}
	
	}

};
```

Time complexity: O(M + N)
Space Complexity: O(1)

