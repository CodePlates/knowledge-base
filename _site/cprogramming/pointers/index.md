# Pointers In C 

Pointers are one of the most dreaded topics in C but also one of the most useful.
This tutorial attempts to explain the basics of pointers so you can gain better understanding and make best use of them. 

## What is a pointer?
A pointers is simply a variable that stores the address of another variable.
Variables are stored in memory locations that have addresses. 
When you declare a variable in a C program, the compiler sets aside a memory location with a unique address to store that variable. The compiler associates that address with the variable's name. When your program uses the variable name, it automatically accesses the proper memory location.
for example
```c
int num = 10;
```
this creates a variable “num” at a certain location in memory. To see that location you use “&” before the variable name  
```c
printf("Address of num: %p\n", &num);
```

output will be something like

**Address of num: 0x7ffcf8949fb4**

##Declaring pointers and assigning values
Declaring a pointer follows the same conventions as regular variables except you use “*” before the variable name

`typename *ptrname;`

```c
int *ptr_num;
```

This creates a variable ptr_num that will be used to hold the address of an integer variable.
 Pointers take addresses as their values. To assign values to a pointer
```c
//initialization
 typename *ptr_name = &variable; 
// assignment
 ptr_name = &variable;
 ```

## Using Pointers
When using pointers, the indirection operator (*) comes into play. When the * precedes the name of a pointer, it refers to the variable pointed to.
Lets try out an example  

```c
#include <stdio.h>

int main()
{
	int num = 10;
	int *num_ptr = &num;

	printf("The value of num is:\t\t %d\n", num);	
	printf("The value of num from num_ptr:\t %d\n\n", *num_ptr);

	printf("The address of num:\t\t %p\n", &num);
	printf("The value of num_ptr:\t %p\n", num_ptr);
}
```

The \t is simply a tab character for formatting reasons. From the output you see that the values are the same depending on how you use it.  
 Note: a pointer is also a variable so &ptr_name will give you the address of the pointer variable itself. 
 Since it’s a variable you can also point to its address with a pointer to a pointer
 typename **ptr_name;
 we will see some examples of this with strings and arrays

## Arrays and Pointer arithmetics

An array is a collection of elements of the same datatype is contiguous memory locations.
 You should already know about arrays and how to use them.
 The interesting thing about arrays is the array variable is also actually a pointer to the first element in the array
int num_arr[10] = { 2, 3, 4, 5, 6 };
num_arr variable contains the memory address of the first element which is 2
```c
printf("First element is: %d\n", num_arr[0]);
printf("Value of *num_arr is: %d\n", *num_arr);
 
printf("Address of num_arr[0]:\t %p\n", &num_arr[0]);
printf("Value of num_arr is:\t %p\n", num_arr);
```
From the output you see that they are the same. And for sure
 num_arr holds address of num_arr[0]
 num_arr  is  &num_arr[0]
 *num_arr  is  num_arr[0]
 
Knowing this, introduces an interesting concept of pointer arithmetic.
 Since arrays are in contiguous memory locations, if say an integer takes 4 bytes
 num_arr[1] will be at location num_arr + 4
 num_arr[2] will be at location num_arr + 4*2
You don’t actually need to know the size a datatype takes up in memory to be able to do pointer arithmetic because the compiler knows and does all that for you
 therefore
num_arr + 1 will be location of num_arr[1]
 (Infact that is why array indices start at 0, so that the index can be used as an offset to quickly and efficiently retrieve data)
*(num_arr + 2) is the same as num_arr[2]
 
You can also do subtraction.
 The compiler actually expands num_arr[3] into *(num_arr + 3) so you can infact do something like 3[num_arr] and it would still work because 3[num_arr] would be expanded to *(3 + num_arr)
 try it out. Its not a lie. 
 As cool as it may seem, I think its best to just forget about pointer arithmetic so that you don’t run into problems of trying to access memory locations that don’t exist. You know about it. Now forget it.

## Strings
A String as you already know, is an array of characters ending with null character.  
All the knowledge from arrays applies, however strings present an issue of mutability. 
 Simply put immutable means it cannot be changed while mutable means it can be changed

```c
char *str = "Immutable string";
```
the data “Immutable string” is declared in data section and cannot be changed. The pointer however can be changed to point to something else and when that happens you may lose access to “Immutable string” and it will stay there lonely with no one to call it.  

```c
char str[] = "Mutable string";
```
this time the data can be changed. It seems weird but it’s very important to remember. 
 
It is difficult to explain but think of it like this.
 When you declare a variable, memory is allocated and space is reserved for that variables data.
 When you declare a pointer, the memory location is only able to hold an address.
For example if addresses are of size 4 (they are actually like 64 bytes but just follow along), an integer could be of size 8. 
 An integer variable will be of size 8 and an integer pointer variable will be of size 4. addresses are of the same size.
`char str[] = "Mutable string";` 
 this creates a character array variable and the contents of the string are stored in the array

```c
char *str = "Immutable string";
```
 this creates a character pointer variable and since the string has no space allocated to it, it is stored in a part of your programs memory called the data section and the address to it is stored in the pointer. Data in the data section cannot be changed. 
 This works for strings because they were designed that way. Don’t just try it on other data types.
```c
//this will not work
 int *num_arr = {0, 1, 2, 3, 4}
```
 To demonstrate immutability, Lets try changing “hello” to “jello”
```c
char str[] = "hello";
str[0] = 'j';   

printf("%s\n", str);
```
As expected the first character ‘h’ is replaced with ‘j’. Doing *str = ‘j’; would yield same result. With immutable strings it would fail miserably with a segmentation fault.
```c
char *str = "hello";
str[0] = 'j'; 
 
printf("%s\n", str);
```
Such a seg fault is a runtime error which is not caught at compile time. Thats why developers like to use const on immutable variables.  
```c
const char *str = "hello";
str[0] = 'j'; 
 
printf("%s\n", str);
```
The above generates a compile time error which is what we like to see. not seg faults. Get into the habit of using const for immutable strings.
The elements of the immutable string cannot be changed, however, the pointer can be changed to point to something else.
```c
const char *hubby = "wifey";
hubby = "sidechick";
```
“sidechick” just replaced “wifey” and now shes lonely with no way to access her anymore.
 What do we do when we don’t want to lose access to “wifey”? We use constant pointers. 
 The previous const was pointers to a constant.  
```c
// pointers to a constant
 const char *hubby = "wifey";

//constant pointer
 char *const hubby = "wifey";
```
With a constant pointer, Now he cant leave her.  

## Functions:  
You should be familiar with functions and passing by value. Passing by value means the value of the variable is passed into the function and not the variable itself.
 
```c
int increment(int number) {
	return (number + 1);
}
int num = 5;
increment(num);
```
there are times when we wish to change the original value outside of the function. With pointers we can just pass the address of the variable to be changed 
 
```c
void magicfix(int *number) {
	*number = 12;
}
int num = 5;
magicfix(&num);
```
This is usually done to improve efficiency and save memory. Instead of passing large values around, instead pass their address. Really effective for strings and structs.
 You notice it with functions like scanf

```c
scanf("%d", &num);
scanf("%s", str1);
```
Yes a string is sort of a pointer.

## Function Pointers
Much like variables, functions also have addresses so you can have pointers to functions. Function pointers follow a stranger format though because in addition to return type, you also have to specify the function parameter types.

`returntype (*func_ptr_name) (parameter_type …)`
 
Lets try out an example.
```c
int sleep(int hours)
{
	printf("%d hours of sleep can save the soul\n", hours);
	return 0; 
}

int main()
{
	int (*nap) (int);
	nap = sleep;
	nap(15);
}
```
This enables you to achieve call back functionality where you pass a function address to a function and it calls it or many other things you can think of
