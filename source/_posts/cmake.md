---
title: Modern Cmake
katex: false
mathjax: false
date: 2024-05-08 00:00:26
tags:
cover_image: /imgs/coverImgs/cmake.jpeg
---
# Basic 

`Cmake`: an open source, cross-platform build tool for most common programming languages.

`Basic Workflow` 
- write an **CmakeLists.txt** file
- create the *build* dir and generate makefile with command: *cmake -B build* 
- compile all the source with command: *cmake --build build* 

`Note:` 

- Cmake has encapsulated the underlying commands to compile the source code, no matter you're using makefile, ninja in unxi system or mingw, msvc in windows, you can use *cmake --build build* to run the compilation.

- Default *Generator* in different system is different:
  
Windows takes **MSVC** as default, if you want to generate makefile, you should specify generator in the command:
```sh
cmake -S <path/to/source> -B<path/to/build> -G "Unix Makefile"
# or you can use
cmake -S <path/to/source> -B<path/to/build> -G "MinGW Makefile"
```
While MacOS/Linux takes **Unix Makefile** as default generator.

# Install

There are many ways you can download cmake depending on which system you're using.

- Windows:
```sh
scoop install cmake
```
- MacOS:
```sh
brew install cmake
```
- Linux: 
```sh
sudo apt install cmake
```
You can also download the cmake with [Official package](https://cmake.org/download/). 

# Better Project Structure


**function**

function(<funcName> [<args> ...])
    # ...
endfunction()

use the function
funcName(<agrs> ...)

# Examples

# References

* [Official Document](https://cmake.org/cmake/help/latest/) 

* [An Introduction to Modern CMake](https://cliutils.gitlab.io/modern-cmake/) 

* [The process of c/cpp files to executable file](https://zhuanlan.zhihu.com/p/556641581) 

* [Do's and Don'ts](https://cliutils.gitlab.io/modern-cmake/chapters/intro/dodonot.html) 
