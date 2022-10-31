---
description: '=nil; Foundation''s Cryptography Suite'
---

# Crypto3

Crypto3 is a cryptography suite of libraries written in modern C++.

There are many libraries which allow developers to integrate cryptographic primitives in their applications. Some well known examples of this include [crypto++](https://www.cryptopp.com/) , [libsodium](https://github.com/jedisct1/libsodium), [OpenSSL](https://github.com/openssl/openssl) ,[Tink](https://github.com/google/tink).

A common theme with these libraries is that they are largely targeting primitives which are widely used and are not fully featured ;certainly not on the bleeding edge of newer applications of applied cryptography. You might find one library implements ECDSA signatures but may not have BLS signatures or might not support **weighted threshold** signing. This forces individual developers/projects to start rolling out their own implementations/libraries , which are targeted for their own use cases. Forking existing implementations ex : [here](https://github.com/filecoin-project/bellperson) , [here](https://github.com/cryptonomex/secp256k1-zkp) or [here](https://github.com/libressl-portable/openbsd) results in greater complexity adding overhead in maintenance as each roll-out starts becoming very specific with reliance on teams to provide support.

One might ask why isn’t there isn’t a one-stop shop for primitives ? There is a good reason for this — writing cryptography libraries is a hard complex task. Most libraries like [boost](https://www.boost.org/) also avoid adding newer primitives or lack a cryptographic module as they are harder to review , and are not yet sold on the stability as the libraries need to evolve/patch at a quicker pace than they want to release.

The current web3 landscape moves very quick and there is a constant demand & need to use different schemes, curves, signing primitives etc. Zero knowledge development is even harder to crack due to limited set of libraries.

\=nil;Foundation’s crypto3 library has been in development since 2018 & aims to fill this gap. It is not just a library, it is a suite, which consists of 17 libraries, representing every major field of modern applied cryptography. It comprises of : VDFs, signature schemes (including threshold with various DKGs), zero-knowledge proof systems (R1CS and PLONK-based ones), more traditional cryptography notions (block ciphers, hashes, message authentication codes, key derivation functions etc).

crypto3  suite aims  to provide :

1. Secure, fast and architecturally clean C++ generic cryptography schemes implementation.
2. Developer-friendly, modular suite, usable for novel schemes implementation and further extension.
3. Standard Template Library-alike C++ interface and concept-based architecture and implementation.
4. Easier prototyping of  novel schemes/proof systems/hashes/other by keeping the Implementation as close to formal constructions.\


crypto3 is used extensively in all applications which =nil;Foundation is developing. This includes our zk-proof system placeholder. Database management protocol and zk bridges.

The repository for crypto3 library is located [here](https://github.com/NilFoundation/crypto3). The suite is highly modular and all components have their own repository ex : [algebra](https://github.com/NilFoundation/crypto3-algebra/), [block](https://github.com/NilFoundation/crypto3-block), [pubkey](https://github.com/NilFoundation/crypto3-pubkey). The core repository is an umbrella repository which adds all the modules under it as sub-modules.

