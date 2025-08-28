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

# Compilers and Linkers
The code that you write is called the source code. 
Compilers translate the high level language to machine language. 

The compiler converts the source file into object code. 

A real program will probably have to reference other code/files. For example, printing to the screen in C and C++ requires the stdio.h, or iostream. The compiler puts a reference to those functions, and the linker links the object code with that library. 

Examples of compilers:
gcc, clang, mcvs
# Increment and Decrement Operators

```C++
int x = 3;
int y = 2 * x++;
int z = ++y * 4;
```

x++ will evaluate the original value of x, then increment it.  ++y first increments the value of y, then evaluates it.