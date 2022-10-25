---
description: '=nil; Foundation''s Cryptography Suite (libcrypto3)'
---

# Crypto3



#### **Why crypto3**?

There are many libraries which allow developers to integrate cryptographic primitives in their applications. Some well known examples of this include [crypto++](https://www.cryptopp.com/) , [libsodium](https://github.com/jedisct1/libsodium), [OpenSSL](https://github.com/openssl/openssl) ,[Tink](https://github.com/google/tink).&#x20;

A common theme with these libraries is that they are largely targeting primitives which are widely used and thus are not fully featured ;certainly not on the bleeding edge of newer applications of applied cryptography. Hence you might find one library implements ECDSA signatures but may not have BLS signatures or might not support **weighted threshold** signing. This forces individual developers/projects to start rolling out their own implementations/libraries , which are targeted for their own use cases.&#x20;

One might ask why isn’t there isn’t a one-stop shop for primitives ? There is a good reason for this — writing cryptography libraries is a hard complex task. Most libraries like [boost](https://www.boost.org/) also avoid adding newer primitives or lack a cryptographic module as they are harder to review , and are not yet sold on the stability as the libraries need to evolve/patch at a quicker pace than they want to release.

The current web3 landscape moves very quick and there is a constant demand & need to use different schemes, curves, signing primitives etc. Zero knowledge development is even harder to crack due to limited set of libraries.

\=nil;Foundation’s crypto3 library has been in development since 2018 & aims to fill this gap. It is compliant with C++ 17 standard.&#x20;

The purpose of crypto3 cryptography suite:

1. To provide a secure, fast and architecturally clean C++ generic cryptography schemes implementation.
2. To provide a developer-friendly, modular suite, usable for novel schemes implementation and further extension.
3. To provide a Standard Template Library-alike C++ interface and concept-based architecture implementation.

The repository for crypto3 library is located [here](https://github.com/NilFoundation/crypto3). The library is modular and all components have their own repository ex : [algebra](https://github.com/NilFoundation/crypto3-algebra/), [block](https://github.com/NilFoundation/crypto3-block), [pubkey](https://github.com/NilFoundation/crypto3-pubkey) .

The core repository is an umbrella repository which adds all the modules under it as submodules.





Cryptography suite can be used as follows:

1. Generic.
2. Selective.

The suite is used as a header-only and is currently statically linked. Future versions will allow dynamic linking.

**Generic**

Generic usage of cryptography suite consists of all modules available at [GitHub =nil; Crypto3 Team Repositories](https://github.com/orgs/NilFoundation/teams/nil-crypto3/repositories).

The generic module can be added to your c++ project as follows

`git submodule add https://github.com/NilFoundation/crypto3.git <dir>`

**Selective**

Developer can select to include a one or more modules to reduce the sources of resulting project and dependencies tree height. This however does require the developer to manually resolve all required dependencies and stay upto date regarding compatibility across modules.

Selective modules can be added to your project as follows:

`git submodule add https://github.com/NilFoundation/crypto3-<lib>.git <dir>`



