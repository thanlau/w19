---
num: "Lecture 4"
desc: "Namespaces"
ready: true
lecture_date: 2019-01-16 09:30:00.00-7:00
---

# Namespaces
* The purpose of a namespace is to avoid <b>naming collisions</b>.

A high-level scenario:
* std::string is a class in the standard library
* We've seen how we can specify the this by using `std::string` or `using namespace std`.
* But what if we are writing a sewing simulator and we want to write our own class called `string` that represents an actual string with a length.
* The C++ compiler might be confused if we don't specify between our string implementation and the standard library's string implementation.

## Example

```
// F1.h
#ifndef F1_H
#define F1_H
#include <iostream>
namespace CS_32_F1 {
	void someFunction() {
		std::cout << "F1.someFunction()" << std::endl;
	}
}
#endif
```
```
// F2.h
#ifndef F2_H
#define F2_H
#include <iostream>
namespace CS_32_F2 {
	void someFunction() {
		std::cout << "F2.someFunction()" << std::endl;
	}
}
#endif
```
```
#include "F1.h"
#include "F2.h"
int main() {
	using namespace CS_32_F1;
	someFunction(); // F1.someFunction()
	return 0;
}
```
```
# Makefile
CXX=g++

# note F1.h and F2.h are not required, but can list them
# to make sure the files exist.
main: main.o F1.h F2.h
	${CXX} -o main -std=C++11 main.o
clean:
	rm -f *.o main
```

<b>Notes:</b>
* using namespace CS_32_F1 prints “F1.someFunction()”
* using namespace CS_32_F2 prints “F2.someFunction()”
* using namespace CS_32_F1; using namespace CS_32_F2;
// ERROR - naming collision!
* using CS_32_F1::someFunction prints “F1.someFunction()”
* using CS_32_F2::someFunction prints “F2.someFunction()”

## Global Namespace
* By default, all variables and functions are defined in a global namespace if no namespace is explicitly defined.

## Appending to existing namespaces
* Namespace definitions can span multiple files (it adds on to the namespace if it already exists).
* Imagine if the standard library had to be defined in a single namespace block!!

## Example - Namespaces spanning multiple files

```
// F1.h
#ifndef F1_H
#define F1_H
#include <iostream>

namespace CS_32_F1 {
    class string {
        public:
        void someFunction();
    };
}
#endif
```
```
// F2.h
#ifndef F2_H
#define F2_H
#include <iostream>

namespace CS_32_F2 {
    class string {
        public:
        void someFunction();
    };
}
#endif
```
```
// F1.cpp
#include <iostream>
#include "F1.h"

using namespace std;

namespace CS_32_F1 {
	void string::someFunction() {
		cout << "F1.someFunction()" << endl;
	}
}
```
```
// F2.cpp
#include <iostream>
#include "F2.h"

using namespace std;

namespace CS_32_F2 {
	void string::someFunction() {
		cout << "F2.someFunction()" << endl;
	}
}
```
```
// main.cpp
#include "F1.h"
#include "F2.h"

int main() {
	CS_32_F2::string x;
	x.someFunction(); // prints “F2.someFunction()”
	return 0;
}
```
```
# Makefile

#CXX=g++

main: main.o Person.o F1.o F2.o
	${CXX} -o main -std=C++11 main.o Person.o F1.o F2.o

clean:
	rm -f *.o main
```

## Specifying something in the global namespace.
* It’s possible that local definitions have namespace conflicts if using namespace ... has a naming conflict.
* You can specify the global namespace by prepending “::” to indicate the global namespace.

## Example

```
// main.cpp
using namespace CS_32_F1;

void someFunction() {
	cout << "in some function (global namespace)" << endl;
}

int main() {
	// someFunction(); // which one?? – won’t compile.
	::someFunction(); // knows it’s someFunction in global namespace
}
```
