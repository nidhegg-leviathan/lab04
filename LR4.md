## Homework
Вы продолжаете проходить стажировку в "Formatter Inc.".

В прошлый раз ваше задание заключалось в настройке автоматизированной системы **CMake**.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в прошлый раз. Настройте сборочные процедуры на различных платформах:
* используйте `GitHub Actions` для сборки на операционной системе **Linux**.





1. Создаём директорию `.github/workflows` и добавляем в неё файл `cmake.yml`:


```yaml
name: CMake_Build

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: checkout
      uses: actions/checkout@v3
    
    - name: Build a *formatter* library
      run: |
        cmake -H. -B_build
        cmake --build _build
      shell: bash
      working-directory: lab_4/formatter_lib
      
    - name: Build a *formatter_ex* library
      run: |
        cmake -H. -B_build
        cmake --build _build
      shell: bash
      working-directory: lab_4/formatter_ex_lib
      
    - name: Build a *hello_world* application
      run: |
        cmake -H. -B_build
        cmake --build _build
      shell: bash
      working-directory: lab_4/hello_world_application
      
    - name: Build a *solver* application
      run: |
        cmake -H. -B_build
        cmake --build _build
      shell: bash
      working-directory: lab_4/solver_application
```

2. Смотрим на результаты работы `GitHub Actions` во вкладке `Actions`.

![Screenshot](./screenshots/screenshost1.png)