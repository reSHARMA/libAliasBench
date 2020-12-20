<h1 align="center">
  <br>
  <a href="#"><img src="https://i.ibb.co/1QDLHCM/6724a9e5-c67a-4c0c-983f-2e3bdb409214-200x200.png" alt="libAliasBench" width="200"></a>
  <br>
  libAliasBench
  <br>
</h1>

<h4 align="center">Benchmark Alias Analysis implementations for precision and speed</h4>


## Table of Contents

- [Getting Started](#getting-started)
  - [Building from source](#build-from-source)
  - [Using with opt](#using-with-opt)
- [Usage](#usage)
  - [Initialization](#initialization)
  - [Abstracting information from LLVM IR instructions](#abstracting-information-from-llvm-ir-instructions)
  - [Evaluating the results](#evaluating-the-results)
  - [Printing the results](#printing-the-results)

## Getting Started

### Building from source
```sh
$ git clone this_repository.git
$ cd this_repository
$ mkdir build; cd build
$ cmake .. && make
$ make install
```
### Using with opt
* Path to shared libary and headers should be searchable by the compiler
* In your LLVM IR pass:
  * Include the header file, ```AliasBench/Benchmark.h```
  * Use namespace ```BenchmarkUtil```
* Load libAliasBench.so before your pass's shared library
  * ``` opt -load /usr/local/lib/libAliasBench.so -load yourPass.so ... ```

## Usage

### Initialization
Create an instance and use it for extracting benchmark data and evaluations
```cpp
...
#include "AliasBench/Benchmark.h"
...
using namespace BenchmarkUtil
...
BenchmarkUtil::BenchmarkRunner Bench;
```

* ```AT``` is an object of ```AliasTokens``` class and should be unique to a module. It store all the tokens for a single module
* ```getAliasToken``` returns alias token from ```AliasTokens``` either by creating a new one or using the already existing one.

### Abstracting information from LLVM IR instructions
It will react to specific function calls and will extract out information for its arguments.
```cpp
...
#include "AliasBench/Benchmark.h"
...
using namespace BenchmarkUtil
...
BenchmarkUtil::BenchmarkRunner Bench;
// Inst is the pointer to a LLVM Instruction 
auto BenchVars = Bench.extract(Inst); 

if(BenchVars.size() == 2) {
    // We found a specific call we care about
}
```

### Evaluating the results
Use the function evaluate for evaluating the results.
```cpp
...
#include "AliasBench/Benchmark.h"
...
using namespace BenchmarkUtil
...
BenchmarkUtil::BenchmarkRunner Bench;
// Inst is the pointer to a LLVM Instruction 
auto BenchVars = Bench.extract(Inst); 

if(BenchVars.size() == 2) {
    Bench.evaluate(Inst, PointeeSet(BenchVars[0]), PointeeSet(BenchVars[1]));
}
```

### Printing the results
```cpp
std::cout << Bench;
```
