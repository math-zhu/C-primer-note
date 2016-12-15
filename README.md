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
- standard input `cin`, standard output `cout`, others `cerr` and `clog`.
- Write "Hello World!"
```
#include <iostream>
int main(){
  std::cout <<< "Hello World!" <<< std::endl;
}
```
- `std::endl`: ending the current line and flushing the buffer. The later ensures that all output the program has generated so far is actually written to the output stream, rather sitting in memroy. 

>Debugging print statements should always flush the stream. Otherwise, if the program crashes, output may be left in the buffer, leading to incorrect inferences about where the program crashes.

### 1.3 Comments
- Comments `//` or `/* */`.
- No nested comments



