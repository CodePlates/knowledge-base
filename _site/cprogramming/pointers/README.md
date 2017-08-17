# Pointers In C 

Pointers are one of the most dreaded topics in C but also one of the most useful.
This tutorial attempts to explain the basics of pointers so you can gain better understanding and make best use of them. 

## What is a pointer?
A pointers is simply a variable that stores the address of another variable.
Variables are stored in memory locations that have addresses. 
When you declare a variable in a C program, the compiler sets aside a memory location with a unique address to store that variable. The compiler associates that address with the variable's name. When your program uses the variable name, it automatically accesses the proper memory location.
for example
```
int num = 10;
```
this creates a variable “num” at a certain location in memory. To see that location you use “&” before the variable name  
```
printf("Address of num: %p\n", &num);
```

output will be something like

**Address of num: 0x7ffcf8949fb4**
