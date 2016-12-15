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
- `std::endl`: ending the current line and flushing the buffer. The later ensures that all output the program has generated so far is actually written to the output stream, rather sitting in memroy. 

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
    }
    return 0;
}
```





