## Problem
You are given a **large integer** represented as an integer array `digits`, where each `digits[i]` is the `ith` digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading `0`'s.

Increment the large integer by one and return _the resulting array of digits_.

**Example 1:**

**Input:** digits = [1,2,3]
**Output:** [1,2,4]
**Explanation:** The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].

**Example 2:**

**Input:** digits = [4,3,2,1]
**Output:** [4,3,2,2]
**Explanation:** The array represents the integer 4321.
Incrementing by one gives 4321 + 1 = 4322.
Thus, the result should be [4,3,2,2].

**Example 3:**

**Input:** digits = [9]
**Output:** [1,0]
**Explanation:** The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].

**Constraints:**

- `1 <= digits.length <= 100`
- `0 <= digits[i] <= 9`
- `digits` does not contain any leading `0`'s.


## Solution 1

Loop through array from end to beginning (digits.length -1 to 0)
	if digits[i] + 1 != 10:
		increment digits[i]
		return digits
	digits[i] = 0 if it does not hit the above code block
	if i == 0
		insert at beginning of digits a 1
		return digits

```C++

class Solution {

	public:
	
		vector<int> plusOne(vector<int>& digits) {

			for (int i = digits.size() - 1; i >= 0; i--) {

				if (digits[i] + 1 != 10) {

					digits[i] += 1;

					return digits;

				}

				digits[i] = 0;

				if (i == 0) {

					digits.insert(digits.begin(), 1);

					return digits;

				}

			}

		return digits;

	}

};
```
Time Complexity: O(N)
Space Complexity: O(1)

## Solution 2
A more convoluted way of doing the above code

```C++
class Solution {

	public:
		vector<int> plusOne(vector<int>& digits) {
			for (int i = 0; i < digits.size(); i++) {
				if (i != digits.size() - 1) {
					continue;
				}
				if (digits[i] + 1 != 10) {
					digits[i] += 1;
					return digits;
				} else {
					int j = i;
					while (j >= 0) {
						if (digits[j] + 1 != 10) {
							digits[j] += 1;
							return digits;

						}
						digits[j] = 0;
						if (j == 0) {
							digits.insert(digits.begin(), 1);
							return digits;
						}
						j--;
					}
			}
		}
		return digits;
	}

};
```
Time Complexity: O(N)
Space Complexity: O(1)