## Problem
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**

**Input:** s = "()"

**Output:** true

**Example 2:**

**Input:** s = "()[]{}"

**Output:** true

**Example 3:**

**Input:** s = "(]"

**Output:** false

**Example 4:**

**Input:** s = "([])"

**Output:** true

**Example 5:**

**Input:** s = "([)]"

**Output:** false

**Constraints:**

- `1 <= s.length <= 10^4`
- `s` consists of parentheses only `'()[]{}'`.


## Solution
Using a stack:
Traverse through the string s. if the s[i] is an opening parenthesis, then push it to the stack. 
If it is a closing parenthesis:
	if the stack is empty, return false
	if not:
		char top = stack.top
		if the top does not match s[i], return false
		else pop the stack
If by the end if the stack is empty, then it is balanced. if it is not, the function will return false.

```C++
class Solution {

	public:
	
	bool isValid(string s) {
	
	stack<char> st;
	
	  
	
		for (int i = 0; i < s.length(); i++) {
	
			if (s[i] == '(' || s[i] == '[' || s[i] == '{') {
	
				st.push(s[i]);
	
			}
	
	  
	
			else if (s[i] == ')' || s[i] == ']' || s[i] == '}') {
	
				if (st.empty()) {
	
					return false;
	
				}
	
				char top = st.top();
	
	  
	
				if ((s[i] == ')' && top != '(') || (s[i] == '}' && top != '{') || s[i] == ']' && top != '[') {
	
					return false;
	
				}
	
	  
	
				st.pop();
	
		}
	
	}
	
	  
	
		return st.empty();
	
	}
	
};

```

Time Complexity: O(N)
Space Complexity: O(N)