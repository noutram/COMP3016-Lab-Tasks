# Lab 1 - C++ Fundamentals
## Introduction
In this module, we will be using the OpenGL (Open Graphics Library) programming interface with the C++ language. For this reason, this lab will give an introduction to C++ with assumed understanding of other general purpose object oriented programming languages, such as C# & Java.

## Setup
All labs will utilise Visual Studio 2019 or 2022 with Windows 10 or 11. Run Visual Studio and click 'Create a new project' & create a project of type 'Console App' with the tags C++, Windows & Console.

If this does not appear, then run the Visual Studio Installer. Find the relevant Visual Studio installation, select 'Modify.' Next, select the 'Desktop development with C++' package & select 'Modify' at the bottom right of the window in order to add the package. Once the package is installed, proceed to run Visual Studio to create the relevant project.

If your project contains no sample code, nor has a .cpp file, navigate to this repository's equivalent project in Lab1/Solutions/Output & use it instead.

**Create Empty C++ Project**
![Create New Project](createNewProject.png)

**Modify Visual Studio Installation**
![Modify](modify.png)

**Install Relevant C++ Package**
![Component](component.png)

## Files
### Types
In your newly created project, a .cpp file should have been generated; this is where the majority of code will be located. This is adequate for the existing contents of this project, however in order to correctly declare object and global variables & functions, a header file must be created. To do this, navigate to the solution explorer, right click the Source Files folder, hover over Add & select New Item. Give the new file the same name as your .cpp file, but give it the .h extension.

### Includes
In order for your .cpp file to be able to make use of your .h file, you must type ```#include "fileName.h"``` at the top of your file. You may have also noticed that the line ```#include <iostream>``` also exists in the project & instead uses angle brackets, as opposed to quotation marks. Depending upon which is chosen will determine how the preprocessor searches for the file to include. Generally, the quotations are used to locate program-related & user-defined header files, whereas the angle brackets are used to locate system header files.

## Input & Output
### Output
To print text to a console, the ```cout``` object can be used. The example project you created in [Setup](#setup) should already demonstrate this as such:

```c++
int main()
{
    std::cout << "Hello World!\n";
}
```

| Task | |
| - | - |
| 1. | Create new C++ Hello World Console Application in Visual Studio |
| 2. | Modify the main function as shown above |
| 3. | Build and run so confirm it is working |
| 4. | Add a second line to underline the string above it |
| 5. | Set a breakpoint on the first line and step through the code |
| | 

Learning to use the debugger in Visual Studio is an important skill. 

### Input
To allow the user to input text to a console, the ```cin``` object can be used. Using ```cin``` will halt the program from running until a user gives input to the console and presses enter:

```c++
int main()
{
    std::cout << "Provide a number!\n";

    int number;
    std::cin >> number;
    std::cout << "Your number: " << number << "\n";
}
```

## Namespaces
### Overview
You have may noticed that both ```cout``` & ```cin``` are prefixed & as such ```std::cout``` & ```std::cin``` are written instead. This is because by default, both ```cout``` & ```cin``` are members of the ```std``` namespace. If the namespace is not specified as a prefix, the C++ compiler will not know what ```cout``` & ```cin``` refer to and the code will not compile.

Namespaces are groups of code, known as declarative regions. Their purpose is to isolate identifiers away from regions of code not under the same namespace. Examples of identifiers could be names for objects, functions & object variables. For example, this allows for a function outside of a namespace to be given the same name as an entirely separate function within another namespace:

```c++
//This will not run in your project, as neither of these namespaces have been declared.
int main()
{
    namespace1::Hello();
    namespace2::Hello();
}
```
Namespaces also help avoid *name collisions*. Another library (with it's own namespace) could have a method called `cout`

Without namespaces, this would not be possible due to the C++ compiler (and linker) being unable to resolve the full method name. The code would fail to build as a result.

| Experiment |
| - |
| Try removing the namespace `std::` from code above so that it reads `cout << "Provide a number!\n";` | 
| What happens when you build? |
| On a separate line, type `std::` and observe the behavior of the editor |

Intellisense is able to narrow the search for method names that belong to the `std`  namespace.

> The `::` symbol is known as the *scoping operator*
>
> We will also see this later when defining methods (known in C++ as member functions) that are members of a class

### Default
For the case where a particular namespace is being used more frequently than another within a cpp file, the namespace can be effectively hidden if one declares it as the default namespace for that cpp file. While the use of the namespace prefix may still be used for the given namespace, it no longer has to be used. Of course, you introduce risk of naming collisions:

```c++
#include <iostream>

using namespace std; //The 'std' namespace will now be assumed

int main()
{
    cout << "Provide a number!\n";

    int number;
    cin >> number;
    cout << "Your number: " << number << "\n";
}
```
## Memory
### Addresses and Values
All data is stored in memory as a combination of a value & an address. The purpose of the address is to know the location of its associated value in memory. Therefore, whenever a variable is read or written to, the variable's address is accessed first to direct the program to the value.

If desired, the address itself can also be directly retrieved & used as opposed to the reference being used solely as a route to acquire the associated value. The ampersand ```&``` is used as a prefix upon a variable in order to acquire its address. Try executing this program to compare the results when printing ```originalNumber``` & `&originalNumber`:

```c++
int main()
{
    int originalNumber = 64;
    cout << "Value: " << originalNumber << "\n";
    cout << "Reference: " << &originalNumber << "\n";
}
```

When printing ```originalNumber```, the value of 64 should be printed. However, when printing ```&originalNumber```, the result should be something like ```000000A12ECFF994```. This is the address. The result of printing the address may change in each instance depending upon the location of the value.

### Copying
In C++, when you copy a variable, you have the option to either copy it entirely (copy by value / deep copy) or only copy its address (copy by reference / shallow copy). Since the reference points to the same value as the preceding variable, the same result will be acquired. In order to hold a reference, the use of the ampersand ```&``` as a suffix to the variable type is used:

```c++
int main()
{
    int originalNumber = 64;
    int& referenceToNumber = originalNumber; //The '&' is used to declare a reference
    int copyOfNumber = originalNumber;
    cout << "Copy by Reference: " << referenceToNumber << "\n";
    cout << "Copy by Value: " << copyOfNumber << "\n";
}
```

One benefit of copying by reference is that it is faster than copying by value, as only the address must be copied as opposed to the entire variable. However, copying by reference is not always applicable. If the value that an address points to is modified, all references will point to the modified one. This is because there is only one value to point to. Try executing the following code to compare the difference between what ```referenceToNumber``` & ```copyOfNumber``` print:

```c++
int main()
{
    int originalNumber = 64;
    int& referenceToNumber = originalNumber;
    int copyOfNumber = originalNumber;

    originalNumber = 128;

    cout << "Copy by Reference: " << referenceToNumber << "\n";
    cout << "Copy by Value: " << copyOfNumber << "\n";
}
```

When ```originalNumber``` is modified, ```referenceToNumber``` mirrors this change. However, ```copyOfNumber``` does not. This is because the former points to the original value, whereas the latter points to a new, copied value.

### Passing
When declaring a function, any parameters must also specify whether to copy by reference or value. For example, the code below achieves the same result as in the previous example, however ```originalNumber``` is now passed through a function. As one might expect, passing by reference through a function is faster than passing by value as less must be transferred. If a reference is passed, the original variable is brought into the function's scope & therefore can be operated upon normally:

**Header**
```c++
#pragma once
void Hello(int& number0In, int number1In);
int originalNumber;
```

**CPP**
```c++
int main()
{
    originalNumber = 64; //Assigned as an object variable for purpose of demonstration
    Hello(originalNumber, originalNumber);
}

void Hello(int& number0In, int number1In)
{
    originalNumber = 128;
    cout << "Copy by Reference: " << number0In << "\n"; //No '&' needed
    cout << "Copy by Value: " << number1In << "\n";
}
```

### Pointers
As opposed to a reference, a pointer is a variable that contains a value that represents the address contained within another variable. To declare a pointer variable, the data type of the variable that it points to must be used, along with an asterisk ```*``` suffix to the type specifier. Alternatively, the ```*``` may be applied as a prefix to the variable name. To instantiate the pointer, the address of the variable that it points to must be assigned. Execute the following code to see what is printed:
```c++
int main()
{
    int originalNumber = 64;
    int* pointerToNumber = &originalNumber;
    cout << pointerToNumber << "\n";
}
```

When ```pointerToNumber``` is printed, the address of ```originalNumber``` should be displayed. However, in order to use the pointer's value as an address to locate & print the value of ```originalNumber```, the pointer must be dereferenced. To dereference a pointer, the ```*``` must be used as a prefix on the pointer's name. Execute the following code & compare what is printed by ```pointerToNumber``` & ```*pointerToNumber```:

```c++
int main()
{
    int originalNumber = 64;
    int* pointerToNumber = &originalNumber;
    cout << pointerToNumber << "\n";
    cout << *pointerToNumber << "\n";
}
```

#### Array Indexing
Pointers can also be used as an index to access an array. This is because when a pointer to an array is created, it assumes the address of its first index. Data within an array is stored linearly in memory. Therefore, the pointer can be incremented in order to shift its memory address to the next element of the array it points to. The example below loops through an array normally. Your task is to modify this code in order to use a pointer to index through the array:

```c++
#include <iostream>

using namespace std;

int main()
{
    const int numbersSize = 4;
    int numbers[numbersSize] = { 64, 128, 256, 512 };

    for (int i = 0; i < numbersSize; i++)
    {
        cout << numbers[i] << "\n";
    }
}
```

### Allocation
When creating a new object, there are two options: static or dynamic allocation of memory. Static memory allocation uses the relatively small stack memory, whilst dynamic memory allocation uses the relatively large heap memory.

Stack memory is allocated when a program is executed and is deallocated upon its closure. For this reason, all statically assigned objects will persist throughout the program's lifetime & do not require manual deallocation. However, heap memory is allocated during the runtime of the given program & therefore dynamically assigned objects must be manually & appropriately deallocated during runtime.

In the example shown, two instances of the class `Dragon` are created. ```Dragon1``` is statically assigned & ```Dragon2``` is dynamically assigned. Static allocation allows for the retrieval of the object itself, whereas dynamic allocation requires that a pointer to the object is instantiated instead. Additionally, ```Dragon2``` must be deleted with the ```delete``` keyword after its use ceases, in order to avoid a memory leak:

**Header**
```c++
#pragma once

#include <string>
using namespace std;

class Dragon
{
public:
    Dragon(string typeIn);
    string type;
};
```
**CPP**
```c++
int main()
{
    Dragon Dragon1("TrueDragon");
    Dragon* Dragon2 = new Dragon("Wyvern");
    delete Dragon2; //If not deleted, will continue to exist after going out of scope
}
```

A pointer to a dynamically assigned object can be used in order to retrieve a member of the object. In this case, the use of the arrow ```->``` is required, whereas the dot ```.``` is used for retrieving members of statically assigned objects:

**Header**
```c++
class Dragon
{
public:
    Dragon(string typeIn);
    string type;

    void Roar();
};
```

**CPP**
```c++
int main()
{
    Dragon Dragon1("TrueDragon");
    cout << Dragon1.type << "\n";
    Dragon1.Roar();

    Dragon* Dragon2 = new Dragon("Wyvern");
    cout << Dragon2->type << "\n";
    Dragon2->Roar();
    delete Dragon2;
}
```
### Null Pointers
Unlike passing or accessing by reference, pointers are variables. Therefore, their lifetime may persist beyond the existence of the variable that it points to. This is problematic in the case of dynamically allocated memory. If a pointer that no longer points to another variable is accessed, it is referred to as a null pointer. Try running this program & observe how it functions incorrectly:

**Header**
```c++
class Dragon
{
public:
    Dragon(string typeIn);
    string type;

    void Die();
};
```

**CPP**
```c++
int main()
{
    Dragon* Dragon2 = new Dragon("Wyvern");
    Dragon2->Die(); //Deallocates itself within function, leaving empty pointer
    cout << Dragon2->type << "\n"; //This will not print the Dragon2 type
}

void Dragon::Die()
{
    cout << type << " died!" << "\n";
    delete this; //deallocation of itself (Dragon2)
}
```