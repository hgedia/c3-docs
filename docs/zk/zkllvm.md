---
description: Compile proof systems from high-level programming languages
---

# zkLLVM



## =nil; zkLLVM Project

zkLLVM is compiler from high-level programming languages into input for provable computations protocols. It can be used to generate input for any arbitrary zero-knowledge proof system or protocol, which accepts input data in form of algebraic circuits It assumed to be used together with `placeholder` proof system or any arithmetization compatible with `placeholder`.

It is designed as an extension to LLVM tool chain , thus supports any front end which compiles to LLVM IR. This enables developers to write code in native language instead of DLS's specific to other libraries.

zkLLVM tool chain adds extensions via:&#x20;

1. `clang` : This compiles the program into intermediate representation byte-code.
2. `assigner` : This creates the execution trace (assignment table).

### Flow

Below we look at flow of how the zkLLVM tool chain is invoked:

1. Users who wish to generate a provable circuit, will write their code in a compatible front end ex: C++. This code will be compiled with the modified version of the `clang` compiler , which will output byte-code representation of the circuit. This step does require that the circuits be defined using cryptographic constructs using crypto3 library. (TODO CHECK)
2.  Users who wish to generate a proof for the circuit , will require to run the `assigner` along with the circuit generated above and pass the public inputs & witness (if necessary). This will output two files:

    * Constraint : Binary file representing arithmetization of the circuit.
    * Execution Trace: Binary file representing the execution trace of the instantiation of the circuit.

    The constraint and the execution trace generated above should be passed as in input to state proof generator binary. (TODO add more details). This will output a binary proof file.
3. Users who wish to verify the proof will be required to feed the proof generated into a script which executes the assigner along with the proof and public inputs to output a json (TO CHECK). This creates data to be passed into EVM contract (or any `placeholder` verifier implementation(TODO- Check)

## Build

### Unix

#### Install Dependencies

* [Boost](https://www.boost.org/) >= 1.74.0
* [cmake](https://cmake.org/) >= 3.5
* [clang](https://clang.llvm.org/) >= 14.0.6

On \*nix systems, the following dependencies need to be present & can be installed using the following command

```
 sudo apt install build-essential libssl-dev libboost-all-dev cmake clang git
```

#### 1. Clone repository

Clone the repository and all sub-modules

```
git clone --recurse-submodules git@github.com:NilFoundation/zkllvm.git
cd zkllvm
```

#### **2. cmake configuration**

```bash
cmake -GNinja -B ${ASSIGNER_BUILD:-build} -DCMAKE_BUILD_TYPE=Debug -DLLVM_ENABLE_PROJECTS=clang .
```

**3. Build clang**&#x20;

```bash
ninja -C ${ASSIGNER_BUILD:-build} assigner -j$(nproc)
```

**4. Build assigner**

```
ninja -C ${ASSIGNER_BUILD:-build} assigner -j$(nproc)
```

## Execute

```bash
${ASSIGNER_BUILD:-build}/libs/circifier/llvm/bin/clang samples/sha512.cpp -emit-llvm -c -O1 -o samples/sha512.bc
${ASSIGNER_BUILD:-build}/bin/assigner samples/sha512.bc -i samples/sha512.inp
```
