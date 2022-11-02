# Implementation



We expanded Boost.Multiprecision with `modular_adaptor`, which is actually a multi-precision number by some modular. It contains modular number-specific algorithms using Montgomery representation. It also supports compile-time computations, because it gives us opportunity to implement algebra constructions as constexpr.

For crypto3 we needed to use field and curve arithmetic in compile time, which became possible thanks to compile-time `modular_adaptor`.

Algebra library consists of several modules listed below:

1. Fields arithmetic
2. Elliptic curves arithmetic
3. Pairings on elliptic curves
4. Multi-exponentiation algorithm (will be part of some other module after a while)
5. Matrices and vectors

### &#x20;<a href="#curves_architecture" id="curves_architecture"></a>

### Pairing Architecture <a href="#pairing_architecture" id="pairing_architecture"></a>

Pairing module consist of some internal functions and frontend interface templated by Elliptic Curve.
