## Problem:
Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example 1:**

**Input:** nums = [2,7,11,15], target = 9
**Output:** [0,1]
**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2:**

**Input:** nums = [3,2,4], target = 6
**Output:** [1,2]

**Example 3:**

**Input:** nums = [3,3], target = 6
**Output:** [0,1]

**Constraints:**

- `2 <= nums.length <= 10^4`
- `-109 <= nums[i] <= 10^9`
- `-109 <= target <= 10^9`
- **Only one valid answer exists.**

**Follow-up:** Can you come up with an algorithm that is less than `O(n2)` time complexity?


### Brute Force
Loop through the array until it reaches the end, *i*
On each iteration of loop, loop through the array starting one index position over until it reaches the end, *j*
if nums[i] + nums[j] == target, those are the positions


```C++
vector<int> twoSum(vector<int>& nums, int target) {


	for (int i = 0; i < nums.size(); i++) {
		for (int j = i + 1; j < nums.size(); j++) {
			if ((nums[i] + nums[j] == target)) {
				return {i, j};
			}
		}
	}
	
	return {}; //empty array if nothing is found
}
```

Time Complexity: O(N^2)
Space: O(1)
### A more efficient method

Trading space for time, utilize a hash map to keep a key, value pair of each element in the array and it's position in the array. Then check if the complement, target - nums[i], exists in the map. If it does, then return that index along with the map[complement]. 
```C++
vector<int> twoSum(vector<int>& nums, int target) {

	std::unordered_map<int, int> mp;

	for (int i = 0; i < nums.size(); i++) {
		mp[nums[i]] = i;
	}

	for (int i = 0; i < nums.size(); i++) {

		int complement = target - nums[i];

		if (mp.find(complement) != mp.end() && mp[complement] != i) {
			return {i, mp[complement]};
		}

	}

	return {};

}

```

Time Complexity: O(N)
Space Complexity O(N) - the map's size will be the same size as nums
