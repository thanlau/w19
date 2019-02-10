---
num: "Lecture 8"
desc: "std::map, std::unordered_map, Testing"
ready: true
lecture_date: 2019-02-06 09:30:00.00-7:00
---

# Maps

* A map is an associated container containing a key / value mapping.

## Example

```
#include <map>
map<int, string> students; // mapping studentIDs to studentNames

// Use bracket notation for creation
students[0] = "Richert";
students[1] = "John Doe";
students[2] = "Jane Doe";
cout << "students[1] = " << students[1] << endl;
```

## Example using find()

* Similar to a set, .find will look for a specific key and return map.end() if the key does not exist.

```
// Check if a student id exists
if (students.find(1) == students.end()) {
	cout << "Can’t find id = 1" << endl;
} else {
	cout << "Found student id = 1, Name = " << students[1] << endl;
}
```

## Example using <string, double> types

```
map<string, double> stateTaxes;

stateTaxes["CA"] = 0.88;
stateTaxes["NY"] = 1.65;

if (stateTaxes.find("OR") == stateTaxes.end()) {
	cout << "Can't find OR" << endl;
} else {
	cout << "Found state OR" << endl;
}
```

## Example between insert vs. []

* .insert will add a key / value pair to the map.
	* If the key already exists, then .insert() will not replace the existing value.
* [] will map a key to a specific value.
	* If the key already exists, then [] will replace the existing value.

```
#include <utility> // for std::pair

// ...

students.insert(pair<int, string>(2, "Chris Gaucho")); // does not replace
students[2] = "Chris Gaucho"; // replaces
cout << students[2] << endl;
```

## Erasing using iterators

* The .erase() can either erase an item in a map using an iterator location OR a specific key value.

## Example

```
// Erasing by iterator
map<int, string>::iterator p = students.find(2);
students.erase(p); // erases "Jane Doe"

// Erasing by key
students.erase(0); // erases "Richert"

// print out the entire map…
for (map<int, string>::iterator i = students.begin(); i != students.end(); i++) {
	cout << i->first << ": " << i->second << endl;
}
```

## `std::map` vs `std::unordered_map`

* The underlying structure of an std::map is implemented with a balanced tree structure.
    * Useful if you care about the ordering of keys.
* `std::unordered_map` is used similarly to std::map, but the `std::unordered_map` is implemented with hash tables.
    * In the example above, replace all `map` with `unordered_map` and notice they work similary on a high-level.
        * Notice the map's key / value pairs are not printed in sorted order by keys.
    * Better average case performance (O(1)), but data order is not guaranteed when traversing the structure with iterators.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-88nc{font-weight:bold;border-color:inherit;text-align:center}
.tg .tg-pq3e{font-style:italic;border-color:inherit;text-align:left}
.tg .tg-kiyi{font-weight:bold;border-color:inherit;text-align:left}
.tg .tg-uys7{border-color:inherit;text-align:center}
.tg .tg-xldj{border-color:inherit;text-align:left}
</style>
<table class="tg">
  <tr>
    <th class="tg-xldj"></th>
    <th class="tg-88nc" colspan="2">unordered_map</th>
    <th class="tg-88nc" colspan="2">map</th>
  </tr>
  <tr>
    <td class="tg-xldj"></td>
    <td class="tg-pq3e">Average</td>
    <td class="tg-pq3e">Worst-Case</td>
    <td class="tg-pq3e">Average</td>
    <td class="tg-pq3e">Worst-Case</td>
  </tr>
  <tr>
    <td class="tg-kiyi">Lookup</td>
    <td class="tg-uys7">O(1)</td>
    <td class="tg-uys7">O(n)</td>
    <td class="tg-uys7">O(log n)</td>
    <td class="tg-uys7">O(log n)</td>
  </tr>
  <tr>
    <td class="tg-kiyi">Deletion</td>
    <td class="tg-uys7">O(1)</td>
    <td class="tg-uys7">O(n)</td>
    <td class="tg-uys7">O(log n)</td>
    <td class="tg-uys7">O(log n)</td>
  </tr>
</table>

# Testing

## Complete Test
* Testing every possible path in every possible situation.
* Complete tests are infeasible!
* The best we can hope for is trying to approximate a Complete Test by testing various types of cases.
* The more rigorous testing a program can “pass”, the more confidence is gained that the program is “bulletproof” (handles every error as expected).

## Unit Testing
* Testing individual pieces (units) of a program (small and large) to ensure correct behavior.
* Good software practice is structuring your program into modular units.
* Good tests generally cover interesting cases including:
	* Normal Cases: Cases where the input is generally what we expect.
	* Boundary Cases: Cases that test the edge values of possible inputs.
	* Error Cases: Invalid cases (like passing a string instead of an int).
		* C++ catches most of these types of errors during the compilation process.
		* Other languages, such as Python, may need to rigorously check invalid input cases.

## Test Suite
* A program containing various tests confirming certain behavior.
* A good way to <i>automate</i> tests.
	* Very important for large-scale projects where other engineers may make a change that affects functionality of other existing (or new) functionality.
* We've seen testing programs in several of our lab assignments testing students' submissions throughout the quarter.

## Example: Writing our own simple program using tddFuncs, which tests a function that takes four integers and returns the largest

```
#include <iostream>
#include <string>
#include "tddFuncs.h"

using namespace std;

int biggest (int a, int b, int c, int d) {
	int biggest = 0;
	if (a >= b && a >= c && a >= d)
		return a;
	if (b >= a && b >= c && b >= d)
		return b;
	if (c >= a && c >= b && c >= d)
		return c;
	return d;
}

int isPositive(int a) {
	return a >= 0;
}

int main() {
	ASSERT_EQUALS(4, biggest(1,2,3,4));
	ASSERT_EQUALS(4, biggest(1,2,4,3));
	ASSERT_EQUALS(4, biggest(1,4,2,3));
	ASSERT_EQUALS(4, biggest(4,1,2,3));

	ASSERT_EQUALS(4, biggest(4,4,4,4));
	ASSERT_EQUALS(-1, biggest(-1,-2,-3,-4));
	ASSERT_EQUALS(0, biggest(-1,0,-3,-4));

	ASSERT_TRUE(isPositive(1));
	ASSERT_TRUE(isPositive(2));
	ASSERT_TRUE(isPositive(0));
	ASSERT_TRUE(!isPositive(-1));
	ASSERT_TRUE(!isPositive(-20));

	return 0;
}
```

