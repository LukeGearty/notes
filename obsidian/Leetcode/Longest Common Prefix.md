## Problem
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

**Input:** strs = ["flower","flow","flight"]
**Output:** "fl"

**Example 2:**

**Input:** strs = ["dog","racecar","car"]
**Output:** ""
**Explanation:** There is no common prefix among the input strings.

**Constraints:**

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` consists of only lowercase English letters if it is non-empty.

## Solution


1. Handle Empty input, return ""
2. set result = strs[0]
3. compare with each string and shrink result as necessary
	1. start idx = 0, curr = ""
	2. while all three are true:
		1. idx < result.size
		2. idx < strs[i].size
		3. result[idx] == strs[i][idx]
	3. curr += res[idx]
	4. increment idx
4. When loop stops, either a mismatch or one string ends, result is set to curr


Time Complexity: O(N * M)

```C++
class Solution {

public:

string longestCommonPrefix(vector<string>& strs) {

if (strs.empty()) return "";

string res = strs[0];

for (int i = 1; i < strs.size(); i++) {

string curr = "";

int idx = 0;

while (idx < res.size() && idx < strs[i].size() && res[idx] == strs[i][idx]) {

curr += res[idx];

idx++;

}

res = curr;

}

return res;

}

};
```

