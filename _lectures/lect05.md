---
num: "Lecture 5"
desc: "More on Structs, Memory Padding, Quadratic-time Sorting"
ready: true
lecture_date: 2019-01-23 09:30:00.00-7:00
---

# Structs
* Even in C, there was a way to represent complex data types called a “Structure” or struct
* A struct brings together a set of heterogeneous members, similar to a class.
* Structs consist of zero or more members of various types.
	* In C, having a struct without any members is illegal, but in C++ it’s fine.
	* The sizeof an empty struct is 1 byte... Why?
		* Because it ensures two different objects will have different addresses when dealing with pointer arithmetic
	* <b>Note:</b> No difference between class / struct except struct defaults members to public and classes default to private.
	* Once a struct is defined, you can declare variables, use them as return types, pass them in parameters, assign them to another struct of the same type... basically treat them like any type!

# Memory Organization of Classes / Structs
* Strictly typed languages enable the compiler to allocate the appropriate memory for functions and types before runtime.
* In memory, the members are stored sequentially and space is allocated based on the types it contains.
* Therefore, the size of the struct seems like it should be the summation of sizes for all of its members... but not exactly.

## Memory Padding
* The concept of leaving <b>blank space</b> when allocating memory to a struct / class in order to improve access speeds when reading data.
* Data is aligned by the types being stored (see figures below):
    * char types (1 byte) - aligns with 1-byte boundaries
    * short types (2 bytes) - aligns with 2-byte boundaries
    * int types (4 bytes) - aligns with 4-byte boundaries
    * double types (8 bytes) - aligns with 8-byte boundaries
* Time vs. space tradeoff
    * Sacrificing a little bit of memory for faster access times.
    * Depending on your application, simply ordering your members in the struct / class can save memory. May be important for applications running on hardware where memory is scarce.

## Example
```
class X {
    int a;      // 4 bytes
    short b;    // 2
    char c;     // 1
    char d;     // 1
};

int main() {
    X x;
    cout << sizeof(x) << endl;                                  // 8
    cout << "&x = " << &x << endl;                              // 0x7ffee6a167c8
    cout << "&x.a = " << &x.a << endl;                          // 0x7ffee6a167c8
    cout << "&x.b = " << &x.b << endl;                          // 0x7ffee6a167cc
    cout << "&x.c = " << reinterpret_cast<void*>(&x.c) << endl; // 0x7ffee6a167ce
    cout << "&x.d = " << reinterpret_cast<void*>(&x.d) << endl; // 0x7ffee6a167cf
}
```
Class X:

![Padding X](PaddingX.png)

```
class Y {
	char a;     // 1 byte
	int b;      // 4
	char c;     // 1
	short d;    // 2
};

int main() {
    Y y;
    cout << sizeof(y) << endl;                                  // 12
    cout << "&y = " << &y << endl;                              // 0x7ffeed3d4768
    cout << "&y.a = " << reinterpret_cast<void*>(&y.a) << endl; // 0x7ffeed3d4768
    cout << "&y.b = " << &y.b << endl;                          // 0x7ffeed3d476c
    cout << "&y.c = " << reinterpret_cast<void*>(&y.c) << endl; // 0x7ffeed3d4770
    cout << "&y.d = " << &y.d << endl;                          // 0x7ffeed3d4772
}
```
Class Y:

![Padding Y](PaddingY.png)

```
class Z {
    public:
    char a;     // 1 byte
    int b;      // 4
    char c;     // 1
    double d;   // 8
};

int main() {
    Z z;
    cout << sizeof(z) << endl;                                  // 24
    cout << "&z = " << &z << endl;                              // 0x7ffee07c1700
    cout << "&z.a = " << reinterpret_cast<void*>(&z.a) << endl; // 0x7ffee07c1700
    cout << "&z.b = " << &z.b << endl;                          // 0x7ffee07c1704
    cout << "&z.c = " << reinterpret_cast<void*>(&z.c) << endl; // 0x7ffee07c1708
    cout << "&z.d = " << &z.d << endl;                          // 0x7ffee07c1710
}
```
Class Z:

![Padding Z](PaddingZ.png)

* For additional reference, here's a good article that explains this well: [https://www.geeksforgeeks.org/structure-member-alignment-padding-and-data-packing/](https://www.geeksforgeeks.org/structure-member-alignment-padding-and-data-packing/)

# Sorting
* Sorting algorithms are useful in order to optimize certain functionality (such as using binary search, checking for duplicates, ...)
* There are MANY sorting algorithms in existence (even inefficient ones such as [Bogosort](https://en.wikipedia.org/wiki/Bogosort)).

## Quadratic-time sorting algorithms
* Sorting algorithms in O(n<sup>2</sup>) are known as quadratic time algorithms.
* We'll cover three quadratic sorting algorithms:
	* Bubbles sort, Insertion sort, and Selection sort

## Bubble Sort
* Bubble sort compares adjacent items in the array and swaps them if a[i] > a[i+1].
	* This guarantees that the largest (or smallest depending on the direction the bubble is moving the item) will be in the proper place.
		* This needs to be done for each element in the collection.
		* O(n<sup>2</sup>) complexity.
		* A good illustration of how Bubble sort works is shown [here](https://en.wikipedia.org/wiki/Bubble_sort).
* Can be slightly optimized
	* If a swap doesn't occur in an iteration, we know the array is already sorted and no more comparisons need to be made.

## Example
```
void bubbleSort(int a[], size_t size) {
	int temp;
	bool swapped;

	for (int i = size - 1; i > 0; i--) {
		swapped = false;
		for (int j = 0; j < i; j++) {
			if (a[j] > a[j + 1]) {
				// swap
				temp = a[j];
				a[j] = a[j+1];
				a[j+1] = temp;
				swapped = true;
			}
		}
		if (!swapped) {
			// in order
			cout << "i = " << i << endl;
			cout << "already in order... returning" << endl;
			return;
		}
	}
}

void printArray(const int a[], size_t size) {
	for (int i = 0; i < size; i++) {
		cout << "[" << i << "] = " << a[i] << endl;
	}
}

int main() {
	int a[] = {1,2,3,4,5,6,7,8,9,10};
	int b[] = {1,10,2,9,3,8,4,7,5,6};
	int c[] = {2,9,4,7,6,5,8,3,10,1};
	int d[] = {10,9,8,7,6,5,4,3,2,1};
	int e[] = {1};

	bubbleSort(a, 10);
	printArray(a,10);
	cout << "---" << endl;
	bubbleSort(b, 10);
	printArray(b,10);
	cout << "---" << endl;
	bubbleSort(c,10);
	printArray(c,10);
	cout << "---" << endl;
	bubbleSort(d,10);
	printArray(d,10);
	cout << "---" << endl;
	bubbleSort(e,1);
	printArray(e,1);
}
```
## Selection Sort
* From top-down (or bottom-up), look through the array and find the largest (or smallest) element.
* Swap a[i] with a[index_of_largest_value]
* O(n<sup>2</sup>) complexity.
* A good illustration of how Selection Sort works is shown [here](https://en.wikipedia.org/wiki/Selection_sort).

```
void selectionSort(int a[], size_t size) {
	int temp;
	int largestIndex;
	int largest;
	for (int i = size - 1; i > 0; i--) {
		largestIndex = 0;
		largest = a[0];
		for (int j = 1; j <= i; j++) {
			if (a[j] > largest) {
				largest = a[j];
				largestIndex = j;
			}
		}
		temp = a[i];
		a[i] = a[largestIndex];
		a[largestIndex] = temp;
	}
}

int main() {
	int a[] = {1,2,3,4,5,6,7,8,9,10};
	int b[] = {1,10,2,9,3,8,4,7,5,6};
	int c[] = {2,9,4,7,6,5,8,3,10,1};
	int d[] = {10,9,8,7,6,5,4,3,2,1};
	int e[] = {1};

	selectionSort(a, 10);
	printArray(a,10);
	cout << "---" << endl;
	selectionSort(b, 10);
	printArray(b,10);
	cout << "---" << endl;
	selectionSort(c,10);
	printArray(c,10);
	cout << "---" << endl;
	selectionSort(d,10);
	printArray(d,10);
	cout << "---" << endl;
	selectionSort(e,1);
	printArray(e,1);
}
```
