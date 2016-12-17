# C++ primer note
## Chapter 1. The basics
### 1.1. simple stuff
- Return 0 for regular exits
```
int main(){
  	return 0;
}
```
- Return -1 for wrong exits: after running the program, run
`$ echo $?`.
It outputs 255 as the return value. 
- Executing program (See 深入理解计算机原理). Compile command:
```
$ g++ -o prog prog.cc
```
- Run command: ```$ ./prog```

### 1.2 I/O
- C++ standard library: iostream. Two important types: istream and ostream. 
```#include <iostream>```
- standard input `cin`, standard output `cout`, others `cerr` and `clog`. For `cin` and `cout`, use `>>` or `<<` for the right direction.
- Example 1: Write "Hello World!"
```
#include <iostream>
int main(){
  	std::cout << "Hello World!" << std::endl;
  	return 0;
}
```

- Example 2: Add two numbers
```
#include <iostream>
int main()
{
	// prompt user to enter two numbers
	std::cout << "Enter two numbers:" << std::endl; 
	int v1 = 0, v2 = 0;
	std::cin >> v1 >> v2;   
	std::cout << "The sum of " << v1 << " and " << v2
	          << " is " << v1 + v2 << std::endl;
	return 0;
}
```
- `std::endl`: ending the current line and flushing the buffer. The later ensures that all output the program has generated so far is actually written to the output stream, rather sitting in memory. 

>Debugging print statements should always flush the stream. Otherwise, if the program crashes, output may be left in the buffer, leading to incorrect inferences about where the program crashes.

### 1.3 Comments
- Comments `//` or `/* */`.
- No nested comments

### 1.4 Flow of control
- `while` good for unspecific conditions.
- `for` good for specific condition.
- Example 3: Reading an unknown number of inputs
```
#include <iostream>
int main()
{
    	int sum = 0, val = 0;
    	// keep executing the while as long as val is less than or equal to 10
    	while (std::cin >> val) {
        	sum += val;  // assigns sum + val to sum
    	}
    	std::cout << "Sum is " << sum << std::endl;
	return 0;
}
```
- we use ctrl-d for end of file.
- using file redirection to save time on I/O.
```
$ prog <infile >outfile
```

### 1.5 Class
- Example 4(Ex 1.22)
```
#include <iostream>
#include "Sales_item.h"
int main(){
    Sales_item newbook, sum;
    if (std::cin >> sum){
        while (std::cin >> newbook){
            if (newbook.isbn() == sum.isbn()){
                sum += newbook;
            }
            else {
                std::cout << sum << std::endl;
                sum = newbook;
            }
        }
        std::cout << sum << std::endl;
    }
    else {
        std::cerr << "no input" << std::endl;
	return -1;
    }
    return 0;
}
```
- Difference between cout and cerr: use cerr for error messages. Use cout for actual output. cout is buffered, cerr is not, so cout should be faster in most cases. (Although if you really care about speed, the C output functions such as printf tend to be a lot faster than cout/cerr).

## Chapter 2 Variables
### 2.1 Primitive Build-in types
- byte = 8 bits. a word = 32 or 64 bytes. 
- int 64 bytes on my computer.
- bool = `true` or `false`.
- The types `int`, `short` and `long` are all signed. Adding `unsigned` for unsigned types.
- Nonbool types to bool is false only when the value is 0.
- float to int truncating the tail.
- int to unsigned (= unsigned int) put negative to positive int.
- Don't mix signed and unsigned types.
- numbers beginning with `0` are in octal notation; numbers beginning with `0x` are in hexadecimal notation.
- use `4.23e10` or `3.14E2` for float numbers.

### 2.2 Variables
- Initialization is not assignment.
- Default initialization: variables defined outside any function body are initialized to zero. With one exception, variables of build-in types defined inside a function are uninitialized.
- Declaration
``` extern int i;```

### 2.3 References and pointers
- reference: alias
```
int val = 1, &value = val 
```
- initializer must be an object with the right type
``` 
int &ref = 10; // error: RHS must be an object
int val =10;
double &r = ref; // error: RHS different type
```
- Pointers
``` 
int val = 1;
int *p =&val;
std::cout << p << " " << *p << std::endl;
p = nullptr;
```
- Initialize all pointers
- `void *` void pointer hold the address of any object.
- Generally, we use a void pointer to deal with memory.
- `int *p, q;` means that `p` is an int pointer and `q` is an int.
- Pointers to pointers
```
int val = 1024;
int *p = &val;
int *q = &p;
```
- Reference to pointer
```
int *&r = p;
```

### 2.4 Constant qualifier
- `const int buf =256` cannot change its value.
- initialization
```
int i = 37;
const int c = i;
int j = c;
```
- `const` objects are local to a file. To share a `const` object among multiple files, you must define the variable as `extern`.
- A reference to `const` may refer to a nonconstant object.
```
int i = 4;
int &r = i;
const int &s = i;
```
Here we cannot use `s` to change the value of `i`.
- Pointer to `const`
```
const int a = 12;
int *p = &a; // error: p is a plain pointer.
const int *q = &a; // ok
```
- `const` pointers
```
int i = 0;
int *const curErr = &i;
```
- Difference between `const` and `constexpr`? TBA
```
const int *p = nullptr; // p is a pointer to a const int
constexpr int *q = nullptr; // q is a const pointer to a int
```

### 2.5 Dealing with types
- type aliases
```
typedef double wages;
wages salary;
using SI = Sales_item;
SI item1;
```
- pointer example
```
typedef char *pstring;
pstring cstr = 0; // cstr is a pointer to char
const pstring *pp; // pp is a pointer to a constant pointer to char
```
- `auto` type
```
auto i = 0, *p = i;
```
- `auto` ignores top-level `const`.
- `decltype` handles top-level `const` and references.
```
const int a = 0, &c = a, *p = &a;
decltype(a) x = 0; // x is const int
decltype(c) y = x; // y has type const int&
decltype(*p) c =r; // c is int&
```
- decltype(()) returns a ref type.

### 2.6 Defining data structures
- Example 1: Sales_data
```
struct Sales_data{
	std::string bookNo;
	unsigned units_sold = 0;
	double revenue = 0.0;
};
```
- Semicolon at the end of a class definition.
- Members without initializer are default initialized.
- write your own head file
```
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include <string>
struct Sales_data {
	std::string bookNo;
		unsigned units_sold = 0;
			double revenue = 0.0;
			};
#endif
```
- uppercase all processor variables as SALES_DATA_H.
- Example 2: Ex 2.41, here we use the new sales_data.h to rewrite Ex 1.22.
```
#include <iostream>
#include <string>
#include "Sales_data.h"
int main()
{
	Sales_data sale,sum;
	double price = 0;  // price per book, used to calculate total revenue
	if (std::cin >> sum.bookNo >> sum.units_sold >> price) {
			sum.revenue = price * sum.units_sold;
			while (std::cin >> sale.bookNo >> sale.units_sold >> price) {
				sale.revenue = price * sale.units_sold;
				if (sum.bookNo == sale.bookNo) {
					sum.units_sold += sale.units_sold;
					sum.revenue += sale.revenue;
				}
				else {
					std::cout << sum.bookNo << " " << sum.units_sold << " " << sum.revenue << std::endl;
					sum = sale;
				}
			}
			std::cout << sum.bookNo << " " << sum.units_sold << " " << sum.revenue << std::endl;
	}
	else {	
			std::cout  << "(no sales)" << std::endl;
			return -1;
	}
	return 0;
}
```
- Here the key is the bool value 
```
(std::cin >> sale.bookNo >> sale.units_sold >> price)
```

## Chapter 3 Strings, Vectors and Arrays

