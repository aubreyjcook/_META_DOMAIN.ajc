# C++

## Purpose

Workflow for initializing a C++ project in a blank repository. C++ has no built-in package manager, so a project is a compiler plus a build system — **CMake** (recommended) or **Make**.

## Prerequisites

- A compiler: **g++**, **clang**, or **MSVC**.
- **CMake**.

## Workflow (CMake)

### 1. Create CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.16)
project(app CXX)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(app src/main.cpp)
```

### 2. Create the source

```plaintext
project/
├── CMakeLists.txt
├── src/
│   └── main.cpp
└── .gitignore
```

### 3. Configure and build

```bash
cmake -S . -B build      # generate build files into build/
cmake --build build      # compile
```

### 4. Run

```bash
./build/app              # macOS/Linux
build\Debug\app.exe      # Windows (Visual Studio generator)
```

## Notes

- `build/` belongs in `.gitignore`.
- Split declarations into headers (`.hpp`) and definitions into sources (`.cpp`); expose an `include/` directory with `target_include_directories(app PRIVATE include)`.

## See Also

- [Java](java.md) — another compiled-language initialization pattern.
