# CMake

# Зміст

${toc}

# Що таке система збоки і навіщо її використовувати

**Система зборки** - утиліта, яка дозволяє автоматизувати процес складання застосунка.

**Автоматизація складання** - етап процесу розробки програмного забезпечення, що полягає в автоматизації широкого спектра завдань, що вирішуються програмістами в їх повсякденній діяльності.

Включає в себе такі дії, як:
- компіляція вихідного коду в Об'єктний модуль
- збірка бінарного коду в виконуваний файл
- виконання тестів
- розгортання програми на виробничій платформі
- написання супровідної документації або опис змін нової версії

# Make

**make** - утиліта, яка автоматизує процес перетворення файлів з однієї форми в іншу. Найчастіше це компіляція вихідного коду в об'єктні файли і подальша компонування у виконувані файли або бібліотеки.

Утиліта використовує спеціальні make-файли, в яких вказані залежності файлів один від одного і правила для їх задоволення.

Make не підтримується нативно в Windows.

# Autotools

**Autotools**, або система збирання GNU, - це набір програмних засобів, призначених для підтримки переносимості вихідного коду програм між UNIX-подібними системами.

Немає нативної підтримки в Windows.

# Cmake

CMake (від англ. Cross-platform make) - це кроссплатформенная система автоматизації збирання програмного забезпечення з джерельного коду. CMake не займається безпосередньо складанням, а лише генерує файли управління складанням з файлів CMakeLists.txt:

- Makefile в системах Unix для збірки з допомогою make;
- файли projects / solutions (.vcxproj / .vcproj / .sln) в Windows для збірки з допомогою Visual C ++;
- проекти XCode в Mac OS X.

![](../resources/img/cmake/img-1.jpeg)

## Хто використовує CMake

- LLVM/Clang compiler framework
- KDE desktop
- MySQL (and its famous fork MariaDB)
- OpenCV
- Blender
- and many others ...

# Hello World project

Створіть директорію cmakehw. Всередині диреторії створіть CMakeLists.txt і файл main.cpp.

![](../resources/img/cmake/img-2.png)

Вміст main.cpp наступний:

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello CMake";
    return 0;
}
```

Вміст CMakeLists.txt наступний:

```
cmake_minimum_required(VERSION 2.8)

add_executable(main main.cpp)
```

Давайте розбиремо, що ж написано в CMakeLists.txt:
- cmake_minimum_required, вказує мінімальну версію cmake з, якою можна збудувати проект
- add_executable - задає ім'я збудованого виконуваного файла і список джерельних файлів

Для зборки проекту потрібно виконати команду:

```bash
cmake .
```

![](../resources/img/cmake/img-3.png)

Оскільки даний приклад, продемонстровано на системі Linux був згенерований MakeFile, для подальшого будування можна використати команду:
```
make
```

![](../resources/img/cmake/img-4.png)

В результаті наш проект буде збудовано. В робочій директорії зявиться файл main:

![](../resources/img/cmake/img-5.png)

Файл main можна запустити командою:

```bash
./main
```

![](../resources/img/cmake/img-6.png)


# Створення виконуваного фалу в іншій директорії

В минулому прикладі виконуваний файл після зборки з'являвся в корневій директорії проекту, що не є хорошою практикою. Для того, щоб виконуваний файл створювався в іншій директорії можна задати параметр - CMAKE_RUNTIME_OUTPUT_DIRECTORY:

```
cmake_minimum_required(VERSION 2.8)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

add_executable(main main.cpp)
```

Після збирання проекту виконуваний файл буде розміщено в директорії bin:

![](../resources/img/cmake/img-7.png)

# Включення CMakeLists.txt з дочірньої директорії

Зараз в нашому проекті всього один джерельний файл, в реальному проекті, звісно, файлів буде сотні, а то і тисячі. Для групування CMake дозволяє створювати CMakeLists.txt в дочірніх директоріях і включати їх в головний CMakeLists.txt:

Додамо до минулого прикладу директорію src. Створимо в директорії src CMakeLists.txt і перенесимо в неї main.cpp:

![](../resources/img/cmake/img-8.png)

src/CMakeLists.txt:
```
cmake_minimum_required(VERSION 2.8)

add_executable(main "main.cpp")
```

CMakeLists.txt:
```
cmake_minimum_required(VERSION 2.8)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

add_subdirectory("src")
```

# Компіляція і використання бібліотек

Для наступної структури проекту:

![](../resources/img/cmake/img-13.png)

CMakeLists.txt на рівні кореня виглядатиме як:

```
cmake_minimum_required(VERSION 2.8.4)
project(test)
add_subdirectory(component1)
add_subdirectory(component2)

add_executable(test main.cpp)

target_link_libraries(test component1 component2)
```

CMakeLists.txt в дочірніх директоріях будуть всього навсього містити:

```
add_library(component2  STATIC  component2.cpp)
```

Компіляція буде виглядати як:

```
cmake .
make
```

![](../resources/img/cmake/img-14.png)


# CMake і .gitignore

.gitignore файл для CMake - проектів виглядає наступним чином:

```
CMakeLists.txt.user
CMakeCache.txt
CMakeFiles
CMakeScripts
Testing
Makefile
cmake_install.cmake
install_manifest.txt
compile_commands.json
CTestTestfile.cmake
_deps
```

# Cmake на Windows з використанням Visual Studio

У Visual Studio 2015 користувачі Visual Studio можуть використовувати [CMake Generator](https://cmake.org/cmake/help/v3.9/manual/cmake-generators.7.html) для створення файлів проекту MSBuild, які IDE витрачає на IntelliSense, перегляд і компіляцію.

Починаючи з Visual Studio 2017 отримав вбудовану підтримку CMake. Головне, потрібно пересвідчитися, що флажок CMake Tools For Visual Studio був ввімкнений при інсталяції(За замовчуванням вів вімкнений):

![](../resources/img/cmake/img-9.png)

Створити новий проект, який використовує CMake можна у вкладці Visual C++ -> Кросплатформні -> CMake:

![](../resources/img/cmake/img-10.png)

В якості згенерованого проекту ми отримаємо вже знайому нам структуру:

![](../resources/img/cmake/img-11.png)

# Домашнє завдання

Створіть проект за допомогою CMake, який виводить Ваше прізвище та ім'яю Залийте проект на рипозиторій, гілка - lb1.

# Контрольні запитання

1. Що таке автоматизація зборки?
2. Як в CMake досягається кросплатформність?
3. Що в собі містить файл CMakeLists.txt