[![Build Status](https://app.travis-ci.com/Yourmaidishere/lab07.svg?branch=main)](https://app.travis-ci.com/Yourmaidishere/lab07)
# Лабораторная работа №7

  1) Для настройки Hunter-пакета для простого приложения solver мне необходимо для начала создать в папке проекта папку cmake и из нее прописать команду ```
$wget https://raw.githubusercontent.com/cpp-pm/gate/master/cmake/HunterGate.cmake -O cmake/HunterGate.cmake```
  2) Таким образом, в папку cmake загружается файл HunterGate.cmake.
  3) Далее в файл CMakeLists.txt я добавляю строки 
```
include("cmake/HunterGate.cmake")

HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"
    SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"
)
```
Они позволяют докачать недостающие файлы для корректной работы и проверить их целостность.

  4) Далее я дописываю команды
```
hunter_add_package(Boost)

find_package(Boost CONFIG REQUIRED)
```
Они добавляют в проект необходимые файлы.

5) В последней строке я указываю следующее: 
```
target_link_libraries(solver PUBLIC Boost::boost solver_lib formatter_ex)
```
  6) Далее создаю файл .travis.yml:
```
language: cpp
compiler:
- gcc
jobs:
  include:
  - name: "all projects"
script:
    - cmake -H. -B_build
    - cmake --build _build
addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
```
