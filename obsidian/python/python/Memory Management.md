Private heap contains all Python objects and data structures. Managing the private heap is done by the Python memory manager, which has different components to deal with management - sharing, segmentation, preallocation, and caching. Users do not control this.

Garbage Collection: if an object has no references pointing to it, garbage collector removes it from memory

Reference Counting: Every object has a reference counter: how many variables are pointing to that object. When it is created, the counter increases. If it reaches 0, Python automatically deallocates that memory.

https://docs.python.org/3/c-api/memory.html
https://www.geeksforgeeks.org/python/memory-management-in-python/