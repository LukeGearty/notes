# Most Vexing Parse

This is when you try to make an object, but the compiler treats it like you are declaring a function.

Example: 

```C++

#include <iostream>

using namespace std;


struct foo {

	int val = 42;

};

  

int main() {
	foo value();
	cout << value.val;
	return 0;
}
```
In the foo value() line, you intend to declare a foo object with a default constructor. The compiler interprets that as a function.

The compiler gives a warning:

```
**vexing.cpp:12:14:** **warning:** **empty parentheses interpreted as a function declaration [-Wvexing-parse]**

   12 |     foo value();

      |              **^~**

**vexing.cpp:12:14:** **note:** replace parentheses with an initializer to declare a variable

   12 |     foo value();

      |              **^~**

      |              {}

**vexing.cpp:13:13:** **error:** **base of member reference is a function; perhaps you meant to call it with no arguments?**

   13 |     cout << value.val;

      |             **^~~~~**

      |                  ()

1 warning and 1 error generated
```


https://en.wikipedia.org/wiki/Most_vexing_parse
https://www.fluentcpp.com/2018/01/30/most-vexing-parse/
