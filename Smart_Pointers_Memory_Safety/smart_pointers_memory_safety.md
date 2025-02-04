﻿Pointers are used for accessing the resources which are external to the program – like heap memory. So, for accessing the heap memory (if anything is created inside heap memory), pointers are used. When accessing any external resource we just use a copy of the resource. If we make any changes to it, we just change it in the copied version. But, if we use a pointer to the resource, we’ll be able to change the original resource.

Problems with Normal Pointers
Some Issues with normal pointers in C++ are as follows:

Memory Leaks: This occurs when memory is repeatedly allocated by a program but never freed. This leads to excessive memory consumption and eventually leads to a system crash.

 Dangling Pointers: A dangling pointer is a pointer that occurs at the time when the object is de-allocated from memory without modifying the value of the pointer.
 
 Wild Pointers: Wild pointers are pointers that are declared and allocated memory but the pointer is never initialized to point to any valid object or address.
  
 Buffer Overflow: When a pointer is used to write data to a memory address that is outside of the allocated memory block. This leads to the corruption of data which can be exploited by malicious attackers.

```

#include <iostream>
using namespace std;
 
class Rectangle {
private:
    int length;
    int breadth;
};
 
void fun()
{
    // By taking a pointer p and
    // dynamically creating object
    // of class rectangle
    Rectangle* p = new Rectangle();
}
 
int main()
{
    // Infinite Loop
    while (1) {
        fun();
    }
}
```

Smart Pointers

As we’ve known unconsciously not deallocating a pointer causes a memory leak that may lead to a crash of the program. Languages Java, C# has Garbage Collection Mechanisms to smartly deallocate unused memory to be used again. The programmer doesn’t have to worry about any memory leaks. C++ comes up with its own mechanism that’s Smart Pointer. When the object is destroyed it frees the memory as well. So, we don’t need to delete it as Smart Pointer does will handle it.

A Smart Pointer is a wrapper class over a pointer with an operator like * and -> overloaded. The objects of the smart pointer class look like normal pointers. But, unlike Normal Pointers it can deallocate and free destroyed object memory.

The idea is to take a class with a pointer, destructor, and overloaded operators like * and ->. Since the destructor is automatically called when an object goes out of scope, the dynamically allocated memory would automatically be deleted (or the reference count can be decremented).
memory header must included.

Unique Pointers
A unique pointer does not share ownership that means unique_ptr stores one pointer only and will free the resource at the end of the scope.


For checking Diagrams
https://www.geeksforgeeks.org/smart-pointers-cpp/ 

```
#include <iostream>
#include <memory>

int main() {
  auto ptr = std::make_unique<int>(10); // Always construct with make_unique function
} //
```

Shared Pointers
By using shared_ptr more than one pointer can point to this one object at a time. A shared pointer does share ownership, and will only free the resource when there are no other owners counted and it has reached the end of the scope and it’ll maintain a Reference Counter using the use_count() method.
```
#include <iostream>
#include <memory>

int main() {
  auto ptr = std::make_shared<int>(10);

  std::cout << ptr.use_count() << "\n"; // Prints the reference count (1)
  {
    auto ptr2 = ptr; // Reference count is now 2
    std::cout << ptr2.use_count() << '\n'; // Prints the reference count (2)
  } // The ptr2 reaches end of scope, reference count is 1 so resource not freed

  std::cout<< *ptr << "\n";
} // The ptr reaches end of scope, reference count is 0 so resource is freed
```

Weak Pointer
Weak_ptr is a smart pointer that holds a non-owning reference to an object. It’s much more similar to shared_ptr except it’ll not maintain a Reference Counter. In this case, a pointer will not have a stronghold on the object. The reason is if suppose pointers are holding the object and requesting for other objects then they may form a Deadlock. 


```
#include <iostream>
using namespace std;
// Dynamic Memory management library
#include <memory>
 
class Rectangle {
    int length;
    int breadth;
 
public:
    Rectangle(int l, int b)
    {
        length = l;
        breadth = b;
    }
 
    int area() { return length * breadth; }
};
 
int main()
{
    //---\/ Smart Pointer
    shared_ptr<Rectangle> P1(new Rectangle(10, 5));
   
    // create weak ptr
    weak_ptr<Rectangle> P2 (P1);
   
    // This'll print 50
    cout << P1->area() << endl;
 
    // This'll print 1 as Reference Counter is 1
    cout << P1.use_count() << endl;
    return 0;
}
```

