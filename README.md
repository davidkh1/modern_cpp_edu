# modern_cpp_edu
Hands-on exploration of modern C++ features and patterns, focusing on concurrency and performance. This repository contains 400+ executable examples from renowned C++ authors, demonstrating C++11 through C++26 features.

## 🚀 Quick Start

### Using CLion (Recommended)
1. Clone the repository
2. Open the project root in CLion
3. CLion will automatically detect CMakeLists.txt
4. Run any example using one of these methods:
   - Click the green arrow next to any `main()` function (easiest!)
   - Right-click on file → Run (if CMake loaded correctly)
   - Select from the run configurations dropdown (top-right)
   - Press Shift+F10 to run the current configuration


### Using Command Line
```bash
git clone https://github.com/davidkh1/modern_cpp_edu.git
cd modern_cpp_edu
mkdir build && cd build
cmake ..
# Compile with half the available cores 
make -j$(( $(nproc) / 2 ))

# Run any example:
./bin/josuttis_cppstd11/algo/accumulate1
./bin/josuttis_cppstd20/ranges/rangeadapt1
```

## 📚 Repository Structure

### josuttis_cppstd11/ (258 examples)
C++11 Standard Library examples from Nicolai Josuttis' "The C++ Standard Library - A Tutorial and Reference, 2nd Edition"
- **algo/** - STL algorithms (accumulate, find, sort, transform)
- **concurrency/** - Threading, async, futures, atomics
- **cont/** - Containers (vector, map, set, array)
- **fo/** - Function objects and lambdas
- **io/** - Input/output streams
- **regex/** - Regular expressions
- **util/** - Utilities (chrono, smart pointers, tuples)

### josuttis_cppstd20/ (150 examples)
C++20 examples from Josuttis' "C++20 - The Complete Guide"
- **comptime/** - Compile-time programming (consteval, constinit)
- **coro/** - Coroutines
- **format/** - std::format
- **modules/** - C++20 modules
- **ranges/** - Ranges library
- **lib/** - New library features (barriers, latches, semaphores)

### rainer_grimm_modernes_c++/ (24 examples)
Modern C++ concurrency patterns and examples
- Thread synchronization patterns
- jthread examples
- Atomic operations
- Producer-consumer implementations

## 🛠️ Build Configuration

The project uses CMake with C++26 as the default standard (with automatic fallback to C++23/C++20):
- **Automatic example discovery** - All .cpp files are built as separate executables
- **Organized output** - Executables in `build/bin/[section]/[topic]/[example]`
- **Full feature support** - Coroutines, modules, threading enabled
- **Cross-platform** - Works with GCC, Clang, and MSVC

## 💡 Usage Examples

### Running a specific example
```bash
# STL algorithm example
./build/bin/josuttis_cppstd11/algo/copy1

# C++20 ranges example
./build/bin/josuttis_cppstd20/ranges/viewstransform1

# Concurrency example
./build/bin/rainer_grimm_modernes_c++/jthreadJoinable
```

### Finding examples by topic
```bash
# List all coroutine examples
find build/bin -name "*coro*" -type f

# List all atomic examples
find build/bin -name "*atomic*" -type f
```

## 📖 Learning Resources

### Books
1. **C++ Concurrency in Action, 2nd Edition** by Anthony Williams
2. **The C++ Standard Library - A Tutorial and Reference, 2nd Edition** by N. Josuttis, 2012. [Site](https://www.cppstdlib.com/)
3. **C++20 - The Complete Guide** by N. Josuttis, 2022. [Site](https://www.cppstd20.com/)

### Videos
- [Modern C++ Concurrency by Mike Shah](https://youtube.com/playlist?list=PLvv0ScY6vfd_ocTP2ZLicgqKnvq50OCXM&si=W0LSYf-MyUS85gxf) - Comprehensive video series

## ⚖️ License

All example code is copyrighted by the respective authors (Nicolai Josuttis, Rainer Grimm) with educational use permissions. See individual source files for specific copyright notices. 