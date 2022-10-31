---
description: Scaffold project using crypto3 library
---

# Quickstart (Scaffold)

***

We will next set up a development environment for crypto3 projects and be able to run an example.This will enable you to test ideas quickly and further explore the API’s of the suite. The example will use a generic setup described above.

## Install dependencies

### Linux

The following dependencies need to be installed.

> [Boost](https://www.boost.org/) >= 1.74.0

> [cmake](https://cmake.org/) >= 3.5

> [clang](https://clang.llvm.org/) >= 14.0.6

Please execute the below to fetch the packages required or adapt the command to your package manager.

```
sudo apt install build-essential libssl-dev libboost-all-dev cmake clang git
```

#### Get crypto3 scaffold project

```
git clone https://github.com/NilFoundation/crypto3-scaffold.gitcd crypto3-scaffold
```

#### Project structure

The project is an example of generic usage of the suite,adding the whole crypto3 suite as a submodule dependency.

```
root├── libs : submodule including the repository for crypto3 suite├── src  │   ├── bls │   │  │──── src: source for bls signing example.
```

```
```

#### Installing and testing scaffold

* Clone sub-modules recursively

```
git submodule update --init --recursive
```

* Build : The project is built using cmake system.

```
mkdir build && cd buildcmake .. && make
```

* Run executable

```
./src/bls/bls_sig
```

You should see the output `Verified signature successfully` on your console.

#### Conclusion

Congratulations! You now have the environment to start experimenting with the crypto3 suite. You can now explore other [modules](https://github.com/NilFoundation/crypto3/blob/master/docs/manual/modules.html) in the suite.Modules also have examples/tests in their repositories’ ex : [algebra example](https://github.com/NilFoundation/crypto3-algebra/tree/master/example). We are working hard on the improving the [documentation](https://crypto3.nil.foundation/projects/crypto3/pages.html). The linked page can be used a reference for now.

In the second part of this article , we will explore a BLS weighted threshold signing example using the pubkey module.

***

\
