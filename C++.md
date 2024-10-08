# C++
[Click here to go back to main](README.md)
## Quirks of the Language

### Copy/Move 
- The difference between `push_back` and `emplace_back` is that `push_back` only has overloads for `const T&` and `T&&` while `emplace_back` takes in a parameter pack `Arg&&... args` which are forwarded to the constructor of the type the vector is defined to hold 
- R value references can bind to const l value references, so if you try to move a const object into either `push_back` or `emplace_back`, then in reality the copy constructor will be called
- ``std::copy`` will **not** create new elements in the destination container. If new elements need to be created (e.g destination vector needs to be resized), then you need to use a ```std::back_insert_iterator```

### Templates
- TODO: add information about how templated methods need "template dot" syntax due to compiler parsing issues 

### String Manipulation
- ```std::stoi``` -> converts a string to an integer
- String streams can be implictly converted into a boolean based upon their state
- String streams have a ```read``` method that takes as arguments a ```char*``` and num chars to read, allowing you to read from a string stream at the granularity of characters
- String constructor is overloaded for C-style strings (array of chars and null-terminated)
- ```tolower``` converts a character to lowercase according to the C locale if it exists. Otherwise, it is left unchanged. The function returns an integer which is the corresponding ASCII number
- ```std::isalnum``` takes an integer corresponding to an ASCII number and returns a non-zero value if it is alphanumeric, as is defined by the C locale
- ```std::string``` has access and modifier member functions that can make it be treated like a stack of chars
- ```(std::string).substr(size_type pos, size_type count)``` is a method on a string that returns the substring of a string, starting at ```pos``` with ```count``` characters

### STL
- ```std::vector``` has a constructor overload which takes in a ```size_t``` to pre-allocate the size necessary
- ```std::vector::resize``` and ```std::vector``` has a constructor that takes a count and a value (overloaded such that the value type is optional) which will create copies of ```value``` wherever the size has been increased (or in the case of ```std::vector``` construction, creates copies of ```value``` for all fields in initialization)
  - Useful for multi-dimensional vectors!
- ```std::unordered_map``` will default construct an element into the container if you try to compare against a value indexed by a key that is not in the unordered_map
- ```std::max_element(ForwardIt first, ForwardIt last)``` provides an STL function to find the max in a given range. The return value is an iterator.
- When calling ```std::sort```, if you are providing a custom comparator, it must be a **strict order**! Otherwise, the sorting algorithm may go into an infinite recursion

### Optimization
- If you **don't** use a ```constexpr``` as an L-value, it **will** be optimized out, as will ```static constexpr```. However, they are not equivalent if used as an L-value:
  - ```static``` puts the data in ```.rodata``` (read-only) segment but non-static will have fields on the stack
  - Potential performance penalty to have to initialize the structure each time
  - Tldr; ```static constexpr``` is almost always preferred

### Arithmetic
- ```size_t``` is unsigned, so if you compare a negative integer and a ```size_t```, the negative number will be implicitly converted to the corresponding unsigned 2's complement number (very big)


## Misc. Definitions
- `std::move` -> casts to an r-value reference, that is all!
- Parameter pack -> Either a template/function parameter pack depending on when used, they are single parameters which hold 0 or more typed/untyped/template parameters 
  - Syntax is usually `typename/type(like int)t /class ... packname(optional)` for declaring the pack in a template
  -  `packname... pack-param-name(optional)` for a function parameter pack
  - `pattern...` is parameter pack expansion, which expands the pattern to 0 or more patterns (at the pattern must contain at least 1 parameter pack)
- Variadic template -> a template with at least 1 parameter pack
- ```std::back_insert_iterator``` -> output iterator that calls the containers ```push_back``` member function whenever the iterator is assigned to
