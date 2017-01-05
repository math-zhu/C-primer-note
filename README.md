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
- Run command: 
```
$ ./prog
```

### 1.2 I/O
- C++ standard library: iostream. Two important types: istream and ostream. 
```
#include <iostream>
```
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
``` 
extern int i;
```

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


### 3.1 Using declaration
- A `using` declaration lets us use a name from a name space without qualifying the name with the name with a `namespace::` prefix.
```
using std::cin;
```
- A separate `using` declaration is required for each name.
```
using std::cin;
using std::cout;
using std::endl;
```

### 3.2 `string` type
 
- A `string` os a variable-length sequence of characters.
```
#include <string>
using std::string;
```
- Initialization of strings
```
string s1; // default initialization is the emtpy string;
string s2 = "hello";
string s3(10,'a');
```
- read a string: stops at a whitespace
```
string s; 
cin >> s; // read a whitespace-separated string into s
cout << s << endl;
```
- `getline`: stops at enter
```
string line;
while (getline(cin, line))
	cout << line << endl;
```
- `s.empty()` returns a bool value
- `s.size()` returns `string::size_type`, which is an UNASSIGNED type.
```
auto len = s.size();
```
- `+` for string addition. Cannot add two literals. One of them must be a string type.
- Ex 3.5: combine a few words into a large string
```
int main(){
          string total;
          string temp;
          while (cin >> temp)
                  total += temp;
          cout << total << endl;
          return 0;
}
```
- Processing every character without change
```
for (auto c : str)
```
- Processing every character with change
```
for (auto &c : str)
```
- processing some character
```
s[i] // i starts from 0 to s.size()-1
```
- Ex 3.6 delete all punctures
```
#include <iostream>
#include <string>
using std::string;
int main(){
         string str;
         getline(std::cin,str);
         for (auto c:str)
                 if (!ispunct(c))
                         std::cout << c;
         std::cout << std::endl;
         return 0;
 }
```

### 3.3 `Vector` type

- A `vector` is a class template. Basic use
```
#include <vector>
using std::vector;
vector<string> svec;
vector<int> ivec;
vector<Sales_item> s_vec;
vector<vector<int>> file;
```
- initialization
```
vector<string> ivec = {"ab","cd","ef"};
vector<string> ivecc(ivec);
vector<int> ivect(10, -2);
vector<string> svec(10); // ten elements
```
- adding elements to a vector, `push_back` takes a value and pushes it as a new last element onto the back of the vector
```
ivect.pushback(i);
```
- hitogram algorithm
```
vector<unsigned> scorecnt(11,0);
unsigned grade;
while (cin >> grade)
	++scorecnt[grade/10]; // division of two integers returns integral part
```
- subscipt does not add elements
```
vector<int> a;
a[0] = 0; // error: use push_back only
```
- read and print a vector
```
#include <iostream>
#include <vector>
using std::vector;
int main(){
vector<int> ivec;
int t;
while (std::cin >> t){
	ivec.push_back(t);
}
for (auto u:ivec){
	std::cout << u << ' ';
}
std::cout << std::endl;
return 0;
}
```

### 3.4 Iterators
- `iterator` is the index of a `vector`.
```
vector<int> v;
const vector<int> cv;
auto it1 = v.begin();	// it1 has type vector<int>::iterator
auto it2 = v.end();	// it2 has type vector<int>::const_iterator
```  
- `iterator` has type `vector<..>::iterator`
- using `iterator` to go through a string
```
string s = "hello world!";
for (auto index = s.begin(); index != s.end(); ++index){
...
}
```
- binary search code
```
#include <iostream>
#include <vector>
using std::cin;
using std::cout;
using std::endl;
using std::vector;
int main(){
vector<int> a = {1,3,5,6,7,19,22,25,77,101};
int goal;
while (true){
	cout << "\ninput a number you want to search: "<< endl;
if (!(cin >> goal)) {
	break;
}
else {
	auto beg = a.begin(), end = a.end();
	auto mid = beg + (end - beg)/2;
	while (true) {
		if (goal == *mid) {
			cout << "find the number at index " << mid-a.begin()+1 << endl;
			break;
		}
		else if (mid == end) {
			cout << "not found" << endl;
			break; 
		}
		else if (goal < *mid) {
			end = mid;
		}	
		else{ 
			beg = mid + 1;}
		mid = beg + (end - beg)/2;
	}
}
}
return 0;
}
``` 
- Remarks:
a. `vector` initialization uses C++11. Compliling using
```
g++ -std=c++11 b.cc -o b
```
b. mid points get rounddown in C++. That explains
```
end = mid // vs
beg = mid +1
```
c. cannot print `iterator` itself. To print the index, we may use
```
mid - a.begin() + 1;
```
d. The statement `cin >> goal` returns false if type unmatched.
- DO NOT add two iterators. 

### 3.5 Arrays
- initialization
```
int arr[num]; // num here must be a constant 
```
- do not copy array to initialize another array, bacause arrays are more like pointers.
```
int a[]=arr;
```
- Array declarations: type modifiers bind right to left.
```
int *p[10]; 	// p is an array of ten pointers
int (*p)[10];	// p is a pointer of an array
int (&r)[10] = arr; 	// ref for an array
```
- The following explain the close relation between pointers and array
```
int num[] = {1, 2, 3};
int *p = &num[0];
int *p = num;
auto p1(num);	// again, an integer pointer
int *q = num + 2; 	// points at 3
int *beg = begin(num);
int *end = end(num);
```
- the type of `begin(num)` is `ptrdiff_t`. Use `auto` better.
- array allows negative subscript, different than vector.
- use `size_t` to script the array

### 3.6 Multidimensional array
- Two dimensional multiarray is an array of arrays. Outer array gives rows. Inner give columns.
- Range for multidimensional arrays
```
for (auto &row:ia)
	for (auto &col:row){
		col = cnt;
		++cnt;
	}
```
- To use a multidimensional array in a range `for`, the loop control variable for all but the innermost array must be references. The reason is that 
```
auto row:ia
```
will give int pointers instead of an array.
- Visit the array with pointers
```
for (auto p = begin(ia); p != end(ia); ++p){	
	for (auto q = begin(*p); q != end(*p); ++q) {
		cout << *q << ' ';
	}
	cout << endl;
}
```

## Chapter 4 Expressions
- Use `++i` rather than `i++`
- `cout << *p++ << endl;` means that
```
cout << *p << endl;
++p;
```
- Member access operator
 ```
string s = "hello", *p = &s;
auto n = s.size();
n = (*p).size();
n = p->size(); 	// equivalent to the above statement
 ```
- Conditional operator
    ```
    string finalgrade = (grade > 90) ? "fail" : "pass"
    ```
when `cout`, the whole statement needs ().
- expilicit conversion
    ```
    int i,j;
    double slope = static_cast<double>(j)/i;
    void *p = &d;
    double *dp = static_cast<double*>(p);
    ```

## Chapter 5 
- null statement
    ```
    ;
    ```
- Conditional statement
    ```
    if (...){
    }
    else if (){...}
    else 
    ```
- switch
    ```
    switch () {
    case A: ...;break;
    case B: ...;break;
    }
    ```
- Do not forget `break` in swith
- use `default:` label for all other cases
- do-while statement ends with `;`
```do {
} while ();
```
- `throw` statement
    ```
    if (item1.isbn() != item2.isbn())
    	throw runtime_error("Data must refer to the same isbn")
    ```
 here `runtime_error` lives in `stdexcept` header
- `try` statement for exceptional case
```
while (cin >> item1 >> item2) {
	    try {
	        // execute code that will add the two Sales_items
	        // if the addition fails, the code throws a runtime_error exception
	        // first check that the data are for the same item 
	        if (item1.isbn() != item2.isbn())
	            throw runtime_error("Data must refer to same ISBN");
	
	        // if we're still here, the ISBNs are the same
	        cout << item1 + item2 << endl;
	    } catch (runtime_error err) {
	        // remind the user that the ISBNs must match 
			// and prompt for another pair
	        cout << err.what() 
	             << "\nTry Again?  Enter y or n" << endl;
	        char c;
	        cin >> c;
	        if (!cin || c == 'n')
	            break;      // break out of the while loop
	    }  // ends the catch clause
	}  // ends the while loop
	
	return 0;   // indicate success
```
- Warning: writing exceprtional safe code is hard!

