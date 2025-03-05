# Memory Management And Garbage Collection In Python

### How are Python objects stored in memory?

In C, C++, and Java we have variables and objects.  
Python has names, not variables.

A Python object is stored in memory with names and references. 
* A name is just a label for an object, so one object can have many names. 
* A reference is a name(pointer) that refers to an object.


Python objects have three things: Type, value, and reference count. 
* When we assign a name to a variable, its type is automatically detected by Python. 
* Value is declared while defining the object. 
* Reference count is the number of names pointing that object.

### Garbage Collection
Python has two ways to delete the unused objects from the memory

#### 1. Reference Counting
The references are always counted and stored in memory.

Consider the code example below

| Code | Object and its reference Count
|---|---|
| a = 50<br>b = a<br>c = 50| 50 with reference count 3 |
| a=60 | 50 with reference count 2 <br> 60 with reference count 1 |
| a = None| 50 with reference count 2 <br> 60 with reference count 0 and hence object 60 is deleted |
| b = True | 50 with reference count 1 <br> False with reference count 1 |
| del(c)   | 50 with reference count 0 and hence its deleted |


As you can see above, del() statement doesn’t delete objects, it removes the name (and reference) to the object. When the reference count is zero, the object is deleted from the system by the garbage collection.

##### Advantages/Disadvantages of reference counting
- it is easy to implement.
- Programmers don’t have to worry about deleting objects when they are no longer used.
- However, this memory management is bad for memory itself! The algorithm always counts the reference numbers to the objects and stores the reference counts in the memory to keep the memory clean and make sure the programs run effectively.
- Also, **in case of cyclic references reference counting can't work**


#### 2. Generational Garbage Collection
Prior to Python version 2.0, the Python interpreter only used reference counting for memory management

Generational garbage collection is a type of trace-based garbage collection. **It can break cyclic references and delete the unused objects even if they are referred by themselves.**

Python keeps track of every object in memory. 3 lists are created when a program is run. Generation 0, 1, and 2 lists.  

Newly created objects are put in the Generation 0 list. A list is created for objects to discard. Reference cycles are detected. If an object has no outside references it is discarded. The objects who survived after this process are put in the Generation 1 list. The same steps are applied to the Generation 1 list. Survivals from the Generation 1 list are put in the Generation 2 list. The objects in the Generation 2 list stay there until the end of the program execution.

### When Does Python Switch from Reference Counting to GC?
Python switches from reference counting to generational garbage collection when:
 - A cyclic reference is detected (e.g., objects referencing each other).
- The threshold for a generation is exceeded (Python automatically runs GC when too many objects pile up).
  
You can check these thresholds with:
```python
import gc
print(gc.get_threshold())  # Default: (700, 10, 10)
700 → If Gen 0 has over 700 objects, it triggers a collection.
10 → If 10 collections occur in Gen 0, Gen 1 is collected.
```

## Memory Allocation in Python
There are two parts of memory:
* stack memory
* heap memory

The methods/method calls and the references are stored in stack memory and all the values objects are stored in a private heap.

### Stack Memory
The allocation happens on contiguous blocks of memory. We call it stack memory allocation because the allocation happens in the function call stack. The size of memory to be allocated is known to the compiler and whenever a function is called, its variables get memory allocated on the stack.

It is the memory that is only needed inside a particular function or method call. When a function is called, it is added onto the program’s call stack. Any local memory assignments such as variable initializations inside the particular functions are stored temporarily on the function call stack, where it is deleted once the function returns, and the call stack moves on to the next task. This allocation onto a contiguous block of memory is handled by the compiler using predefined routines, and developers do not need to worry about it.

Example:
```
def func():
      
    # All these variables get memory 
    # allocated on stack 
    a = 20
    b = []
    c = ""
```

### Heap Memory
The memory is allocated during execution of instructions written by programmers. The variables are needed outside of method or function calls or are shared within multiple functions globally are stored in Heap memory.

Example:

```
# This memory for 10 integers 
# is allocated on heap. 
a = [0]*10 
```


## GC Module (Garbage Collection) in Python
The gc module in Python provides an interface to the garbage collection facility for automatic memory management. It helps to reclaim memory by collecting objects that are no longer in use (unreachable) to free up space. Python uses reference counting and garbage collection to manage memory.

