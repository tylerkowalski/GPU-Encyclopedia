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

## Misc. Definitions
- `std::move` -> casts to an r-value reference, that is all!
- Parameter pack -> Either a template/function parameter pack depending on when used, they are single parameters which hold 0 or more typed/untyped/template parameters 
  - Syntax is usually `typename/type(like int)t /class ... packname(optional)` for declaring the pack in a template
  -  `packname... pack-param-name(optional)` for a function parameter pack
  - `pattern...` is parameter pack expansion, which expands the pattern to 0 or more patterns (at the pattern must contain at least 1 parameter pack)
- Variadic template -> a template with at least 1 parameter pack
- ```std::back_insert_iterator``` -> output iterator that calls the containers ```push_back``` member function whenever the iterator is assigned to
