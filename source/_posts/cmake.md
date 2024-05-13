---
title: Cmake
katex: false
mathjax: false
date: 2024-05-08 00:00:26
tags:
cover_image: /imgs/coverImgs/cmake.png
---

## Cmake workflow

Cmake: an open source, cross-platform build tool for most common programming languages

**Basic workflow**:
- write **CmakeLists.txt** file, following is the basic structure of it:

```cmake
# minimal version of cmake
cmake_minimum_required(VERSION 3.20)
# project name
project(Hello)
# generate a executable applicatio
# parameters: binary file, cpp files...
add_executable(Hello hello.cpp)
```
- create **build** dir and generate makefile and other files in that dir: **cmake -B build** 
- generate an executable application: **cmake --build build** 

### Cmake in different system
1. Windows:
- prefer directly download **binary files** of cmake, easy to use
- takes **MSVC** as default generator in Windows

2. Linux:
- prefer download **source code** of cmake and manually compile them
- takes **Unix Makefile** as default generator in Windows

## Grammar

cmake command tool is constructed from 5 executable files:
- cmake 
- ctest
- cpack
- cmake-gui
- ccmake

Common grammar in **CmakeLists/.cmake** files:

```cmake
# determine minimal version
cmake_minimum_required()
# print message
message()
# obtain variables in cmake file: ${}, for example, print path
message($ENV{PATH})
# create a new env path in the scope of cmake file
set(ENV{cXX} "g++")
```

### Manipulating varibles

1. Set()/Unset()
- sinlge vlaue: set/Unset(variable value)
- multi values: set/Unset(variable value1 value2)

2. List()

### Workflow control

1. **if control**

```cmake
# all the key words
if()
    # ...
elseif()
    # ...
else
    # ...
endif

# common comparison symbols 
# NOT, AND, OR, LESS, EQUAL
```

2. **loop control**
- foreach

```cmake
# three major ways to use foreach
# 1st
foreach(<variable> RANGE <range>)
    # ...
endforeach

# 2nd
foreach(<variable> IN <List variable> <extra items>)
    # ...
endforeach

# 3rd, zip operatio.>n
foreach(<variable> IN ZIP_LISTS <List1> ...)
    # ...
endforeach
```

- while: **not** recommanded

3. **function**

```cmake
function(<funcName> [<args> ...])
    # ...
endfunction()

# use the function
funcName(<agrs> ...)
```

4. **scope** 


5. **macro** 


## Command

-P .cmake files

-G "compiler name"

## 4 main ways to Build

## Static/Dynamic link lib

## Examples

## Prerequisite

1. C/CPP生成.exe文件流程
- 预处理
- 编译
- 汇编
- 链接

2. Cmake, Makefile and Make
