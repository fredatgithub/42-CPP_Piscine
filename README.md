42-CPP-Piscines
<details>
  <summary>CPP_00-ex00 </summary>

You can use the system function (char)toupper

c

#include <iostream>

int main()
{
    std::cout << "Hello World!" << std::endl;
    return 0;
}

To read or write to the standard input/output stream, you need to include <iostream>. Any variable or object existing in the C++ standard library is included in the standard namespace std. To use a standard output object, you need to write std::cout to append the namespace. A namespace signifies the belonging of a specific object or function. When an object is declared in a() and b(), it means it can only be used with another prefix.

arduino

int doSomething(int x, int y)
{
    return x + y;
}

arduino

int doSomething(int x, int y)
{
    return x - y;
}

c

#include <iostream>
int main()
{
    std::cout << doSomething(4, 3);
    return 0;
}

If these two functions are included in the same program, as in the example, there is a naming conflict because the function with the same name and parameters is in the same scope.

arduino

namespace Foo
{
    int doSomething(int x, int y)
    {
        return x + y;
    }
}

arduino

namespace Goo
{
    int doSomething(int x, int y)
    {
        return x - y;
    }
}

c

#include <iostream>

int main()
{
    std::cout << Foo::doSomething(4, 3) << '\n';
    std::cout << Goo::doSomething(4, 3) << std::endl;
    return 0;
}

If two developers work on a project and both decide to name their function doSomething, there will be conflicts during compilation. Therefore, namespaces can be used. By using namespace Foo and namespace Goo, both doSomething functions will not be aware of each other, and everything will compile successfully. The scope resolution operator (::) is necessary to search for identifiers in each namespace. To use this operator, prefix the identifier name with the namespace.
</details>
<details>
  <summary>CPP_04-ex00 </summary>

c

#include <iostream>
using namespace std; // Just to avoid writing "std::" every time

class Animal {
    public:
    void eat() {
        cout << "Like any other animal, I can eat!" << endl;
    }

    void sleep() {
        cout << "Like any other animal, I can sleep!" << endl;
    }
};

class Dog : public Animal {

    public:
    void bark() {
        cout << "I am a dog and I bark!" << endl;
    }
};

// int main() {
//     Dog dog;

//     dog.eat();
//     dog.sleep();
//     dog.bark();
//     return 0;
// }

//---------------------Example from the video-------------------------//
class Gun{
    public:
        virtual void Shoot() // 'virtual' allows the method to be overridden in a derived class (Uzi)
        {
            cout << "Bang!" << endl;
        }
};

class Uzi: public Gun{
    public:
        void Shoot() // 'override' is ideally written here after the brackets, but it's for the C++11 standard
        {
            cout << "Bang! Bang! Bang!" << endl;
        }
};

int main() {
    Gun gun; // Created an object of the Gun class
    Uzi uzi; // Created an object of the Uzi class, which is derived from the Gun class

    Gun *weapon = &uzi; // 1 - Created a pointer to Gun, which points to uzi
    //Gun *weapon = &gun; // 2 - Created a pointer to Gun, which points to gun

    // This pointer can refer to its own type
    // To an object of the same class
    // Or to any other class that is inherited from it

    weapon->Shoot();
    // If 1, it will be "Bang! Bang! Bang!"
    // If 2, it will be "Bang!"
    return 0;
}

</details>
<details>
  <summary>Simple Encapsulation Example (get, set) </summary>

c

#include <iostream>
using namespace std; // Just to avoid writing "std::" every time

class Point{
    private:
        int _x;
        int _y;
        int _z;
    public:
        int getX(){
            return _x;
        }
        void setX(int valueX){
            _x = valueX;
        }
    void print()
    {
        cout << "X = " << _x << endl;
        cout << "Y = " << _y << endl;
    }
};

int main()
{
    Point a;
    a.setX(5);
    a.print();

    return 0;
}

</details>
<details>
  <summary>Template is like a cupcake mold, but not the cupcake itself. We give the mold to the compiler, and it bakes cupcakes for us </summary>

c

#ifndef WHATEVER_HPP
#define WHATEVER_HPP

//template<comma-separated parameter list>
//The keyword 'typename' defines what is called a type parameter, or for short, a template type parameter.
//template <typename T> = template <class T>
template < typename T >
T max(T a, T b){
    return b < a ? a : b;
    // If b < a, return a; otherwise return b.
}

template < typename T>
void swap(T& a, T& b) {
    T tmp = a;
    a = b;
    b = tmp;
}

template < typename T >
T min(T a, T b){
    return b > a ? a : b;
}

#endif

//The process of substituting template parameters with specific types is called template instantiation. The result of this is an instance of the template.

</details>
<details>
  <summary>CPP_06-ex01 </summary>

c

#include <iostream>

/*reinterpret_cast<type>(expr)
'type' specifies the resulting type of the cast. 'expr' is the expression being cast to the new type.
It converts one type to a completely different type. For example, it can convert a pointer to an integer to an integer and vice versa.
It is the simplest form of casting and forces the compiler to accept situations that 'static_cast' usually would not.
*/

/*
In this task, you are required to convert a pointer to 'Data', which is an arbitrary class or user-defined structure, to 'uintptr_t', and then convert 'uintptr_t' back to a pointer to 'Data'.
In this process, you must ensure that no data is lost, and the data must remain intact.
*/

/*
'uintptr_t' is a typedef of an unsigned integer type. In my case, 'uintptr_t' is 'unsigned long'.
The signed integer equivalent is 'intptr_t', and 'uintptr_t' and 'intptr_t' are identical in the sense that they are used to store the address a pointer references, in numeric form.
On some systems, you might see that 'intptr_t' can be assigned both signed and unsigned, while 'uintptr_t' can only be unsigned and requires separate type conversion for signed.
For this reason, starting from the C language, it's recommended to use 'intptr_t' for program flexibility instead of 'uintptr_t'.
*/

struct Data
{
    int data;
    std::string string;
};

Data* deserialize(uintptr_t raw){
    return reinterpret_cast<Data*>(raw);
}

uintptr_t serialize(Data* ptr){
    return reinterpret_cast<uintptr_t>(ptr);
}

int main(){
    Data data;
    Data *newData = NULL;
    data.data = 42;
    data.string = "My mind is full of sawdust";

    std::cout << "newData: " << newData << "\n";
    std::cout << "data string: " << data.string << std::endl;
    std::cout << "data int: " << data.data << std::endl;

    uintptr_t ptr;
    ptr = serialize(&data);
    newData = deserialize(ptr);

    std::cout << "newData: " << newData << "\n";
    std::cout << "data string: " << data.string << std::endl;
    std::cout << "data int: " << data.data << std::endl;

    return 0;
}

The connection between TCP and UDP is established in a "process-to-process" relationship. After passing through the 3rd layer and completing end-to-end classification, it is determined which machine all the packets corresponding to you are going to, and it is necessary to properly distribute them to their respective processes, which is the primary role of the 4th layer. Process classification is performed using ports in the OS module, and data to be transmitted by each process is recorded in the socket buffer depending on the port, and the OS handles them accordingly, then other processes are handled. Or they will be sent to another machine. What if the address that the pointer refers to is recorded among the contents to be written to the socket buffer? Since other machines cannot know the current user's memory address, it becomes impossible to interpret the data.

Even if you are lucky enough to switch to another process on the same machine, it is a relocation address, so it is difficult to recognize it from another process. Therefore, pointers require separate conversion. This is called serialization.
For example, suppose you have the number 100 in a structure named Data and the address of this value is 0x10. If the Data structure is written without serialization, the recipient only gets 0x10, so it is impossible to obtain the correct value. Therefore, to correctly interpret the value of the Data structure, instead of writing 0x10, we dereference it and write the value 100. Of course, actual serialization is not such a simple conversion of a pointer to a value, but it is necessary to consider other situations such as inheritance between objects and cyclic pointers within objects. To actually create serialization like this, so many concepts are involved, and implementing all the content in this topic is not only nearly impossible but also beyond the scope. Thus, the serialization required in ex01 understands that only its value is used and simply aims to understand reinterpret_cast<T> through simple code that can only be applied to the current process.
</details>
<details>
  <summary>CPP_08-ex00 </summary>

STL (Standard Template Library) is an abbreviation for the Standard Template Library, which includes containers, iterators, and algorithms. As the name suggests, it is a library that applies templates and supports Containers, Iterators, and Algorithms for arbitrary types.
An iterator is an object created to refer to elements in a container such as std::vector and std::pair. As seen from its meaning, it is implemented similarly to how a pointer works. In other words, given that a pointer was used to manage an array in the past, it is managed with an iterator in a Container.
The function called easyfind is a function that finds a specific value in a container. If the specific value is not found, it throws an exception or returns the corresponding value.
In my case, easyfind was understood as a function that finds a value using the std::find function and throws an Exception if the value is not found.
A container is a collection that stores multiple objects of the same type.
An iterator is an object that provides access to each element by iterating over the elements stored in the Container.
</details>
<details>
  <summary>CPP_08-ex02 </summary>

Adapters internally use full-fledged containers like std::vector and std::deque, and adapt their interface to another interface. For example, std::vector has methods like push_back, insert, and pop_back, but such operations are not needed for a stack. A stack only needs three basic operations: top, pop, and push. But all these operations are implemented through the corresponding operations of std::vector, which is hidden inside std::stack. Therefore, a stack is not considered a standalone container—it is an adapter for std::vector.
</details>