# Local Variables & Function Arguments
- Pass by Value: Address Changes from the function call to the param and the assignment
- **Pass by Address**: Address doesn't change anywhere
- Pass by Reference: Address doesn't change from the function call to the param but it does on the assignment
- Pass by Const Reference; Adress doesn't change from the function call to the param and the param cannot be changed. The assignment does change

# Initialization
- Brace Initialization: `int x{5}; std::vector<int> vect{1,2,3,4};` (will fail compilation if trying to convert from another type (such as float))

- Usually NULL is interpretted as an int, but depending on the compiler it could be interpretted as something else
nullptr is interpretted as a pointer

Classes has its members private by default
Structs has its members public by default

# Constructors
- Normal: Initializes properties from the params
- Copy: Initializes properties from another object

# Pointers
- Common memory allocation happens on the **Stack**
- `new` allocates memory on the **Heap**
- **Heap** allocations will be used until the program is closed (if not released)
- "Memory Leak" => Memory that wasn't released at the end of the scope
- `delete` releases the memory allocated with `new`
- `delete[]` should be used only when releasing containers

# Arrays, Strings & Vectors
- Dynamic Arrays allow dynamic size allocation but `[]` doesn't
- C-Style Arrays have a special character `\0` that denotes the end of the string
- `std::string` is a class that holds the character and size data
- `std::string` can be allocated using `{}` and has a destructor that deallocates the memory at the end of the scope
- `std::vector` is pretty similar to `std::string` but for any type

# Conway's Game of Life
- Cellular Automata -> A two dimmensional array where one cell depends on the behavior of its neighbor cells
- Cells are either Alive or Death
- The grid advances cause the cells to either stay alive or die
- Cell Transitions:
    - The cell dies if it has less than 2 alive neighbors
    - The cell dies if it has more than 3 alive neighbors
    - An unpopulated will become alive if it has 3 or more alive neighbors
    - Otherwise the cell will maintin its state

# Two Dimmensional Arrays
- 

# Numeric Types and Literals
- There are integers and unsigned integers, the later used for sizes and structures
- There are some literals that doesn't affect the numbers but are helpful for readability such as `'` for example `int a = 1'000'000;`
- Other literals can name a type. For example `.f` for denoting floats or `ULL` for denoting unsigned long longs.

# String Literals
- `\n` new line literal
- `\"` in-string quote literal
- `R"(String goes here)"` to process everything inside the () as an string without needing `\`
- If the string contains an `()` the delimiter should be x, for example `R"x()x"`

# Casting
- C++98 Cast -> `int a = 'A';` => a = 65 - `(char)a` => a = A
- `static_cast<>` is normal casting -> `static_cast<char>(a)`
- `const_cast<>` is casting from a const expression to a non-const expression
- `reinterpret_cast<>` is for casting from a buffer data to an untyped binary data
- `dynamic_cast<>` is for casting from a base class cast to a child class

# Iterator Introduction
- `begin()` and `end()` returns iterators to the first and last object respectevily
- `end()` is an invalid iterator and should never be dereferenced
- `rbegin()` and `rend()` work the same as the previous ones but in reverse

# auto Keyword
- Is born from the necesity shorten long types and numerous namespaces
- When to use `auto`?
    - When the type does not matter
    - When the type does not provide useful information
    - When the code is clearer without the type
    - When the type is difficult to discover
    - When the type is unknowable

# Loops & Iterators
- `const_iterator` prevent the loop from modifying the object or the container is `const`
- `reverse_iterator` iterates backwards the container
- `cbegin()` and `cend()` are the modern const iterators for the begin and end iterators. Same goes for reverse iterators `crbegin()` and `crend()`

# Iterator Arithmetic and Iterator Ranges
- We can add and subtract to an iterator
- `end() - 1` gives us the last element
- `end() - begin()` gives us the number of elements
- `next()` takes an iterator and returns the following -> `next(array::begin())`
- same for prev, but it returns the previous element -> `prev(array::end())`
- `distance()` returns the number of elements between to iterators -> `distance(array::begin(), array::end())`
- `advance()` moves the iterator certain amount of positions -> `advance(array::begin(), 3)`

# If and Switch statements on C++17
- From C++17 and on is supported the "in-if initialization" -> `if(auto iterator = array::begin())`
- `iterator` is scoped to the conditional
- Same goes for `switch` statements -> `switch(const char c = get_next_char(); c)`
- `[[fallthrough]]` is an attribute that can be placed on an empty `switch` case and tell it that the `break` statement was intentionally omitted

# Templates Overview
- Used for writing code that is functionally the same but operates on differentes data types
- Known as "Generic Programming"
- "Instantiation" is when we use the templating for an specific data type -> `std::vector<int>`
- It happens automatically and is done by the compiler
- When writing a template, we use a dummy type (often referred as "template parameter") to show the compiler what the code looks like
```C++
templater<class T>
T Max(const T& t1, const T& t2)
{
    if(t1 > t2)
        return t1;
    return t2;
}
```
- With a regular function, the compiler only needs to be able to see its declaration when called BUT with a template function, the compiler must be able to see the full implementation when called
- Most programmers write the template definition on the header file so its included automatically
- Some programmers put all their templats in a separate ".inc" file and include that file separately
- On C++98 `typename` was added to replace the `class` keyword on templates

# Namespaces
- Tool to group logically related symbols
- Namespaces are created by using the `namespace` keyword followed by the name of the Namespace -> `namespace abc {...}`
- Every symbol declared inside the namespace will have the namespace's name automatically prefixed to it by the compiler
```c++
namespace abc
{
    class Test;
}
abc::Test test;
```
- If a name is not in any namespace, it is said to be in the "global namespace"
- This is denoted by `::` before the name of the symbol
```c++
class Test
{
    ...
};
::Test test;
```
- Namespaces can be split ober different parts of the code or over different files
- When a symbol is defined in a `namespace`, it "hides" any symbols outside the namespace with the same name
```c++
int x = 23;
namespace abc
{
    int x = 47;
    std::cout << "x = " << x << '\n';       // Prints 47
    std::cout << "x = " << ::x << '\n';     // Prints 23
}
```
- The `using` keyword brings versions of the symbols to the global namespace -> `using namespace std;`

# Function Pointers
- Every function's executable code is stored in memory and we can get a pointer to the address of the start of that code
```c++
void funct(int i1, int i2);
// this could also be defined as => void (*func_ptr)(int, int) = &func;
auto func_ptr = &func;
```
- We can create an alias type for this type of function pointer -> `using pfunc = void(*)(int, int);`
- Function pointers are callable objects, that means they behave like a variable and can be called like a function
- We call the function by dereferencing the pointer -> `(*func_ptr)(1, 2);`
- Function pointers can be passsed as function arguments or returns
- Usefull for writing callbacks
- Ugly syntax