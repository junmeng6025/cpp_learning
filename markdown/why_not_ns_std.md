# Why I don't "using namespace std"
[YT Video](https://www.youtube.com/watch?v=4NYC-VU-svE) @The Cherno  
## 1. What is `namespace`
and what  "using namespace std" actually does.  
e.g.:
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>

void ForEach(const std::vector<int>& values, const std::function<void(int)>& func)
{
    for (int value : values)
        func(value);
}

int main()
{
    std::vector<int> values = {1, 5, 4, 2, 3};
    auto it = std::find_if(values.begin(), values.end(), [](int value) {return value >3; });
    std::cout << *it <<std::endl;

    int a = 5;

    auto lambda = [=](int value) { std::cout << "Value: " << a << std::endl; };

    ForEach(values, lambda);
    std::cin.get();
}
```
Every time using something from standard library, we need type `std::` prefix before it.  
When we declare `using namespace std;`, we don't need the `std::` prefixes any more, the code seems to look cleaner, BUT also harder to indicate, which components are from standard library.  

For example, `vector` might exist in two libraries:
- `std::vector`
- `eastl::vector`

if `vector` comes barely,  we cannot tell which actually are we using.  

```cpp
// namespace 1: print the given string
namespace apple{
    void print(const std::string& text)
    {
        std::cout << "apple output: " << text << std::endl;
    }
}

// namespace 2: print inversly the given char series
namespace orange{
    void print(const char* text)
    {
        std::string temp = text;
        std::reverse(temp.begin(), temp.end());
        std::cout << "orange output: " << temp << std::endl;
    }
}

// declare using namespaces
using namespace apple;
using namespace orange; // this would overwrite last one

// main func
int main()
{
    print("Hello!"); //???
    /***
    here "Hello" is actually a const char array,
    not a string.
    but cpp will do an implicit conversion to string, 
    if needed
    ***/ 

};
```

NOW:
```cpp
// namespace 1: print the given string
namespace apple{
    void print(const std::string& text)
    {
        std::cout << "apple output: " << text << std::endl;
    }
}

// namespace 2: print inversly the given char series
namespace orange{
    void print(const char* text)
    {
        std::string temp = text;
        std::reverse(temp.begin(), temp.end());
        std::cout << "orange output: " << temp << std::endl;
    }
}

// declare using namespaces
// using namespace apple;
// using namespace orange; // this would overwrite last one

// main func
int main()
{
    apple::print("Hello!");
    

};
```
Never ever declare `using namespace xxx` in header file. Who knows where it would be included, and suddenly you're using a space somewhere you didn't originally intend to.  
This problem can be very hard to track when we have issue when compiling / debugging.  
<br>

So, stick on using `std::` has advantages:
- You always know what you are using
- You avoid possible errors in the future

You can use `using namespace xxx` in some smaller scope as possible, like inside an if statement, inside a function, or, maybe in a cpp script...  
BUT NEVER IN A HEADER FILE.