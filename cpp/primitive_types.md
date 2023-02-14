# Primitive types
## Table of contents
* [variable types](#variable-types)
* [Initialization a variable](#initialization-variable)
* [std::string](#string)

## Variable types

Variable type and size in bytes
```
char : 1
short : 2
int : 4 (at least 2)
long int : 4
long long int: 4
float : 4
double: 8
pointer_type* : 4 bytes on 32, 8 bytes on 64
```
```cpp
const char* str = "Hello World"; //immutable
char* ptr = new char{'A'}; // pointer to char
```
The name of the array is the same as the pointer to its first element. Example below.
```cpp
#include <iostream>

void modify(char* ptr, size_t size) {
    for (int i = 0; i < size - 1; i++) {
        *ptr = 'x';
        ptr++;
    }
}

int main() {
    char table[] = "Hello world";
    modify(table, sizeof(table));
    std::cout<<table;
}
```

## Initialization a variable

Good practise is to initialize variable using braces. This protects against implicit conversion
```cpp
int x {5};
instead of:
int x = 5;
int y {5.2}; //error C2397: conversion from 'double' to 'int' requires a narrowing conversion
```
In the case of auto it does not matter. There is no implicit conversion at all.
```cpp
auto x = 5
auto x {5}
```

## std::string

String has a size of 24 bytes (size, capacity and pointer to the data). SSO means small string optimization. For sure on 64bit architecture, string has 40 bytes of size (24 + 16 SSO bytes).\
The SSO mechanism is often used in C++ standard library implementations to improve performance and reduce memory overhead associated with memory allocation and deallocation.
