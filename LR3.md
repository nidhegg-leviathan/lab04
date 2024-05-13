## Homework
Представьте, что вы стажер в компании "Formatter Inc.".




### Задание 1
**Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории [formatter_lib](formatter_lib). В этой директории находятся файлы для статической библиотеки *formatter*. Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib), с помощью которого можно будет собирать статическую библиотеку *formatter*.**


```bash
mkdir formatter_lib
cd formatter_lib

code CMakeLists.txt
```

```cmake
cmake_minimum_required(VERSION 3.4)

project(formatter_lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
```

```bash
mkdir build && cd build
cmake ..
cmake --build .

```




### Задание 2
**У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш  руководитель поручает заняться созданием `CMakeList.txt` для библиотеки  *formatter_ex*, которая в свою очередь использует библиотеку *formatter*.**


```bash

cd ..
mkdir formatter_ex_lib && cd formatter_ex_lib
code CMakeLists.txt 
```

```cmake
cmake_minimum_required(VERSION 3.4)
project(formatter_ex_lib)
set(CMAKE_CXX_STANDART 11)
set(CMAKE_CXX_STANDART_REQUIRED ON)
include_directories("../formatter_lib")
add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
```

```bash
mkdir build && cd build
cmake ..
cmake --build .

```




### Задание 3
**Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой *formatter_ex*, вам необходимо создать два `CMakeList.txt` для двух простых приложений:**
* *hello_world*, которое использует библиотеку *formatter_ex*;


```bash
cd ../
mkdir hello_world_application && cd hello_world_application
code CMakeLists.txt
```

```cmake
cmake_minimum_required(VERSION 3.4)
project(hello_world)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(hello_world hello_world.cpp)

add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)


target_include_directories(formatter_lib PUBLIC ../formatter_lib)
target_include_directories(formatter_ex_lib PUBLIC ../formatter_ex_lib ../formatter_lib)
target_include_directories(hello_world PUBLIC ../formatter_ex_lib ../formatter_lib)

target_link_libraries(hello_world formatter_ex_lib formatter_lib)
```

```bash
mkdir build && cd build
cmake ..
cmake --build .
```

* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

```bash
cd ../
mkdir solver_application && cd solver_application
code CMakeLists.txt
```

```cmake
cmake_minimum_required(VERSION 3.4)

project(solver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)



add_library(formatter_lib STATIC ../formatter_lib/formatter.cpp)
add_library(formatter_ex_lib STATIC ../formatter_ex_lib/formatter_ex.cpp)
add_library(solver_lib STATIC ../solver_lib/solver.cpp)

include_directories(	../formatter_lib
			../formatter_ex_lib
			../solver_lib)


add_executable(solver equation.cpp)

target_link_libraries(solver 	solver_lib 
				formatter_ex_lib 
				formatter_lib)
```

```bash
mkdir build && cd build
cmake ..
cmake --build .
```
