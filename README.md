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

