---
num: "Lecture 16"
desc: "Final Review"
ready: true
lecture_date: 2019-03-13 09:30:00.00-7:00
---

# Final Review
* <b>NOTE:</b> Use these sample exams as additional practice and anticipation of what kind of questions may be asked on the final exam. <b>DO NOT</b> base your studying only on these exams. I expect all students to review the material thoroughly.

* Link to Final from F18:
[F18](http://cs.ucsb.edu/~richert/cs32/exams/F18_FINAL.pdf)
* Link to Final from S18:
[S18](http://cs.ucsb.edu/~richert/cs32/exams/S18_FINAL.pdf)

```
Logistics
- Bring student ID and writing utensil
	- ink or dark led
- Please write LEGIBLY
- No electronic devices
- No book, no notes
- 30% of your final grade

*** FINAL EXAM WILL NOT BE IN PSYCH 1924 ***
- Final exam will take place in Broida Hall (BRDA 1640), 8am - 10am on Wednesday 3/20

Format of the exam
- Similar to midterms, but designed to take two hours (~twice as long)
- Mix of questions
	- Short answer
	- Briefly describe / define /state…
	- Write code
	- Fill in the blank / complete the table (or diagram)
	- True / False, if false explain why
	- Given code, what is the ouput

Topics
- Will cover everything from the start of the quarter up to Tuesday (3/11) lecture on Threads.
- Covers lecture, labs, homework, and reading
	- Use lecture notes as breadth of topics, and all other course material (lecture, hw, labs, reading) as depth.
- Makefiles
- STL (vector, map, unordered_map, string, iostream, thread, unistd.h, …)
- Class Design
	- Abstract Data Types
	- Build process
	- .cpp, .h (linking)
	- Big Three (copy constructor, destructor, assignment operator)
	- Scope Resolution Operators (::)
		- Used in Class method definitions and namespaces
	- Templates
- Structs and Classes
	- Differences
	- Memory storage (hexadecimal notation)
	- Syntax
- Memory Padding
- Namespaces
	- Naming collisions (and how to avoid them)
	- Global namespace
- Quadratic Sorting
	- Bubblesort, selectionsort, insertionsort
	- Optimizations
	- Running times
- Hash Tables
	- Performance and functionality
	- Open-address, double-hashing, chained-hashing (pros / cons)
	- std::map, std::unordered_map (under-the-hood implementation concepts and run time)
- Mergesort and Quicksort
	- Algorithm
	- Running times
	- Pros / cons
- Inheritance
	- Understand concepts and behavior
	- Understand memory allocation of base / sub class types
	- Hierarchy
- Polymorphism
	- Understand concepts and behavior in various scenarios
	- Virtual vs. non-virtual
	- Abstract classes and pure virtual functions
	- Virtual destructors
- Exceptions
	- Try / catch / throw mechanisms and flow control
	- Inherited types and how catches work with types.
- Function Pointers
	- Syntax of function pointers
		- How to declare, assign, and call functions via function pointers
	- Using function pointers with Threads
	- Write code utilizing function pointers
- Testing
	- Understand main ideas and terminology
	- tddFuncs' way of testing
- OS Concepts
	- Role of the OS
	- OS kernel
	- Relationship to software / hardware
	- Processes
	- Threads
	- Basic UNIX commands from lecture notes
		- ps, ps –e, top, jobs, ^C, ^Z, kill, ...
	- Foreground vs. background processes
		- How they relate to the terminal application
	- Fork / exec
		- Example with command line instructions and unistd.h functions in a C++ application
- Heaps
	- MinHeap and MaxHeap
	- Conceptual tree structure
	- Implementation
	- Array representation and index of children / parent of a node
	- Insertion and maintenance
	- Deletion (of root)
	- Runtime analysis
- Threads
	- Threads and hardware (threads running on cores)
		- Concurrency
	- Using <thread> library to create threads
	- .join(), .detach() – what they do and why is this important
	- Race conditions
	- Atomic Operations and Mutex
	- Deadlock
```
