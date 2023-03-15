# How C++ works?
[YT Video](https://www.youtube.com/watch?v=SfGuIVzE_Os).  
How we go from the source file, the actual text file to an executable binary, the program we can run?  
***
The basic workflow of C++ program:
- You have a series of source files, which you write actual text in.
- You pass it through a compiler, which compiles it into some kine of binary.
  <br>  

    >binary can be some sort of library or be an actual executable program

## Start with a `hello world` program
```cpp
// Preprocessor statement
#include <iostream>

int main()
{
    std::cout << "Hello World!" << std::endl;
    std::cin.get();
}
```

## Declaration and Definition of a function
e.g.: Wrapp the `std::cout` function in another func `Log`

```cpp
// main.cpp
#include <iostream>

void Log(const char* msg)  // Declaration of `Log`

int main()
{
    Log("Hello World!");
    std::cin.get();
}
```
```cpp
// Log.cpp
// Definition of `Log`
#include <iostream>

void Log(const char* msg)
{
    std::cout << msg << std::endl;
}
```

- **Declaration:** this function / var exists.
  
    > But how does compiler know we actually have a Log function?  
    > $\to$ The compiler just trusts us.
  
- **Definition:** this is what this function / var is.

How does the compiler find where the definition of the declared function? $\to$ **Linker**  
If the compiler didn't manage to find the function definition, there comes **Linker error**.

- **Linker**: resolve the symbols, wire up functions. 
  
  >When it couldn't find what to wire a declared function to, it comes to error. So the definition is to provide a body to the declared functions.