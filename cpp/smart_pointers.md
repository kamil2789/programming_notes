# Smart pointers
## Table of contents
* [General information](#general-information)
* [std::unique_pointer](#unique_ptr)
TODO!

## General information

1. We do not create smart pointers from raw pointers. It can be done but never do it regardless of the circumstances.

## std::unique_ptr
1. Only movable
2. Takes up as much memory as a regular pointer
```cpp
#include <memory>
#include <iostream>

int main() {
    std::unique_ptr<int> ptr(new int {5});
    std::cout<<*ptr<<"\n";

    std::unique_ptr<int> ptr2 = std::make_unique<int>(20);
    std::cout<<*ptr2<<"\n";

    ptr = std::move(ptr2);
    std::cout<<*ptr;
    //std::cout<<*ptr2; segfault (nullptr)
}
```

## std::shared_ptr

1. They have the size of two unique_ptr pointers;
2. You can't assign an already existing raw pointer to a shared_ptr because you'll mess up the counter;
3. make_shared avoids the cost of dynamic memory allocation. One allocation for the data object and another allocation for the control block. Single chunk of memory;
4. Decreasing and increasing the counter are atomic operations;
5. A constructor called with a raw pointer creates a control block;
```cpp
	auto wsk = new Test{ 44 };
	std::shared_ptr<Test> ptr(wsk);
	std::shared_ptr<Test> ptr2(wsk);
	std::cout << ptr2.use_count(); //exhibits 1.
// deletes memory twice and program crashes
```

Best practise:
```cpp
auto ptr = std::make_shared<int>(100);
auto ptr 2 = ptr //create a second pointer to the data 100
```

## Custom deleter
1. Lambda makes the size of unique_ptr one larger than usual, which means it loses the advantage of not creating an overhead;
2. For shared_ptr, secifying a non-standard deleter does not increase the size of the object, is not part of the pointer, and is part of the control block;
```cpp
#include <memory>
#include <iostream>

class Test
{
public:
	Test(int a) : x(a) {}
	~Test() {
		std::cout << "CLASS DESCTRUCTOR\n";
	}

	void show() const {
		std::cout << x << "\n";
    }

private:
    int x;
};

auto customDeleter = [](Test* test) //always takes raw pointer 
{
	std::cout << "CUSTOM DESTRUCTOR: ";
	test->show();
	delete test; //custom deleter should always take care of free memory
};


int main() {
    std::unique_ptr<Test, decltype(customDeleter)> ptr(new Test{ 44 }, customDeleter);
    std::shared_ptr<Test> ptr2(new Test{120}, customDeleter);
}
```

## Memory leak in cyclic reference std::shared_ptr
None of the following objects will be released due to each pointing to the next one.
```cpp
#include <iostream>
#include <memory>

class Testme
{
public:
    Testme(int data):
        value(data)
    {}

    ~Testme() {std::cout<<"DESTROYED"<<value<<"\n";}
    int getValue() const {return value;}

    std::shared_ptr<Testme> next;
private:
    int value;
};

int main()
{ 
    {
    auto first = std::make_shared<Testme>(300);
    auto second = std::make_shared<Testme>(400);
    auto third = std::make_shared<Testme>(500);

    first->next = second;
    second->next = third;
    third->next = first;

    std::cout<<first.use_count();
    std::cout<<second.use_count();
    std::cout<<third.use_count();
    }
    std::cout<<"END";
} 
```

## Weak pointers - std::weak_ptr
TODO!