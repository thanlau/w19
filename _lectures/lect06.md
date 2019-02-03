---
num: "Lecture 6"
desc: "Midterm 1 Review, Quadratic-Time Sorting cont., Hashing"
ready: true
lecture_date: 2019-01-28 09:30:00.00-7:00
---

* <b>NOTE:</b> Use these sample exams as additional practice and anticipation of what kind of questions may be asked on the midterm exam. <b>DO NOT</b> base your studying only on these exams. I expect all students to review the material thoroughly.
* Link to Midterm 1 from F18:
[F18](http://cs.ucsb.edu/~richert/cs32/exams/F18_M1.pdf)
* Link to Midterm 1 from S18:
[S18](http://cs.ucsb.edu/~richert/cs32/exams/S18_M1.pdf)

```
Logistics
- Bring studentID and writing utensil
- No electronic devices
- No notes, no books

Format
- Will be a mix of questions based on lectures, homeworks, readings, and labs.
- Short Answer
	- Briefly define, describe, state, ...
- Write code (includes makefiles)
- Fill in the blank / complete the table
- Given code, write the output
- Given a statement, tell me if it's true / false
	- if false, briefly state why
- Expect it to take ~ one hour (maybe sooner)
	- but we have the entire class period
- Will cover a broad range of topics, but probably not everything

Topics
- Will cover everything up to Wednesday's lecture (1/23)

Makefiles
- Know how to create one when multiple files are linked to form an
executable
- Know the build process
	- Proprocessing, Compiling, Linking
- Know what the default rules are, flags, special symbols (as discussed in lecture)

Standard Template Library
- Reviewed some details about vectors
	- Operations like [], at, front, back, pop_back, push_back, size, constructors, ...
- STL container iterators
	- begin(), end(), ++, <, *, erase

Class Design
- Abstract Data Types
- How to design an interface (.h file) and its implementation (.cpp)
- Public vs. Private
- Accessors (getters) vs Mutators (setters)
- Constructors (default, overloaded, copy)
- Header guards
- Scope Resolution operator (::) and .cpp method definitions
- Shallow vs. Deep Copy
- Big Three (Rule of Three): copy constructor, destructor, assignment operator

Structs and Classes
- What are the difference(s)?
- Memory Padding (how they are stored in memory)

Namespaces
- Avoid naming collisions
- How to create namespaces in your code
- Global namespace (how to specifiy using "::")

Binary Search
- Recursive (and iterative (non-recursive)) implementation
- Performance (running time), O-notation

Quadratic Sorting Algorithms
- bubbleSort
	- Optimized version
- selectionSort
```

## Insertion Sort
* Similar to sorting cards in a poker hand
* For each item in the array, find where it should "belong" and insert the item in its proper place.
	* Before insertion happens, shift all elements in order to to make an empty slot where the item should belong.
	* Do this for each element, and by the end of the algorithm, all elements will be in their proper place.
	* O(n<sup>2</sup>) complexity.
	* O(n) complexity for elements that are mostly-sorted.

## Example
```
void insertionSort(int a[], size_t size) {

	int item;
	int shiftIndex;
	for (int i = 1; i < size; i++) {
		item = a[i];
		shiftIndex = i - 1;

		while (shiftIndex >= 0 && a[shiftIndex] > item) {
			a[shiftIndex + 1] = a[shiftIndex];
			shiftIndex -= 1;
		}
		a[shiftIndex + 1] = item;
	}	
}

int main() {
	int a[] = {1,2,3,4,5,6,7,8,9,10};
	int b[] = {1,10,2,9,3,8,4,7,5,6};
	int c[] = {2,9,4,7,6,5,8,3,10,1};
	int d[] = {10,9,8,7,6,5,4,3,2,1};
	int e[] = {1};

	insertionSort(a, 10);
	printArray(a,10);
	cout << "---" << endl;
	insertionSort(b, 10);
	printArray(b,10);
	cout << "---" << endl;
	insertionSort(c,10);
	printArray(c,10);
	cout << "---" << endl;
	insertionSort(d,10);
	printArray(d,10);
	cout << "---" << endl;
	insertionSort(e,1);
	printArray(e,1);
```

# Hashing
* The ability to address unique key values in an array whose size may be smaller than the set of possible key values.
* Generally, keys are unique values that identify some record of information.
	* In order to put data (value) in the appropriate place in the collection, a hash function is required.
		* The hash function outputs a position in the collection based on the key value
		* Hash function outputs should be uniformly distributed
			* Or else all data would try to be stored in the same index.
		* For example, if you have array of 100 elements, a simple hash function could be:
			* key % 100 = [0-99]

![Hash Table](hashTable.png)

* Hashing is very efficient for searching for data in an array.
	* Recall binary search: O(log n) search time (if elements are sorted).
	* Linear search: O(n) search time.
	* Hash Table search: O(1) average search time in unsorted order.
		* Hash Table searching provides instant access to an element in an array since the hash function computes the index where the data is stored.

## Collisions
* It's possible that two elements may be indexed to the same location.
	* This is known as collisions

## Open-address Hashing
* Collisions are resolved by placing the item in the next open spot in the array.
* For example, if a record is hashed to position i and data already exists in that index, then check the next available spot i+1, i+2, etc.
	* Known as linear probing
	* What is a problem with this mechanism?
		* Delayed Insertion – inserting an item in a crowded hash table takes time since it must look for an empty spot.
			* Elements are inserted farther away from their actual hashed index.
		* Clustering – When different keys are hashed to the same index, there may be groups of data records grouped in the same place.

![Open Address](openAddress.png)

## Double Hashing
* Technique that uses a 2nd hash function when resolving a collision.
	* If a hash function index results in a collision, then use the 2nd hash function to determine how far to step in the array to look for an empty slot.
	* Helps reduce the clustering effect.
* Problems
	* If hash2 function is large, there is a possibility that we will go out of bounds.
	* Depending on the table size and hash2, it is possible that the index won’t be uniformly distributed.

![Double Hashing](doubleHashing.png)

## Chained Hashing
* The biggest problem to open-address hashing is 
	* If the table is full, no more elements can be added.
	* Similar to a vector, it could expand the capacity “under-the-hood” when needed, but…
		* All elements will probably have to be rehashed
		* New capacity shouldn’t be wasteful (too big) or too small
* Chained hashing (chaining)
	* If a collision occurs, then we store a series of data records in a list that the index in the hash table references.
	* Linked Lists are a common collection to store collided data records.

![Chaining](chaining.png)

