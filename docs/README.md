---
description: '=nil; Foundation''s Cryptography Suite'
---

# Crypto3

## **Why crypto3**?

There are many libraries which allow developers to integrate cryptographic primitives in their applications. Some well known examples of this include [crypto++](https://www.cryptopp.com/) , [libsodium](https://github.com/jedisct1/libsodium), [OpenSSL](https://github.com/openssl/openssl) ,[Tink](https://github.com/google/tink).

A common theme with these libraries is that they are largely targeting primitives which are widely used and thus are not fully featured ;certainly not on the bleeding edge of newer applications of applied cryptography. Hence you might find one library implements ECDSA signatures but may not have BLS signatures or might not support **weighted threshold** signing. This forces individual developers/projects to start rolling out their own implementations/libraries , which are targeted for their own use cases.

One might ask why isn’t there isn’t a one-stop shop for primitives ? There is a good reason for this — writing cryptography libraries is a hard complex task. Most libraries like [boost](https://www.boost.org/) also avoid adding newer primitives or lack a cryptographic module as they are harder to review , and are not yet sold on the stability as the libraries need to evolve/patch at a quicker pace than they want to release.

The current web3 landscape moves very quick and there is a constant demand & need to use different schemes, curves, signing primitives etc. Zero knowledge development is even harder to crack due to limited set of libraries.

\=nil;Foundation’s crypto3 library has been in development since 2018 & aims to fill this gap. It is compliant with C++ 17 standard.

The purpose of crypto3 cryptography suite:

1. To provide a secure, fast and architecturally clean C++ generic cryptography schemes implementation.
2. To provide a developer-friendly, modular suite, usable for novel schemes implementation and further extension.
3. To provide a Standard Template Library-alike C++ interface and concept-based architecture implementation.

The repository for crypto3 library is located [here](https://github.com/NilFoundation/crypto3). The library is modular and all components have their own repository ex : [algebra](https://github.com/NilFoundation/crypto3-algebra/), [block](https://github.com/NilFoundation/crypto3-block), [pubkey](https://github.com/NilFoundation/crypto3-pubkey) .

The core repository is an umbrella repository which adds all the modules under it as submodules.

