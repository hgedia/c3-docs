# Implementation

## Implementation # <a href="#algebra_impl" id="algebra_impl"></a>

@tableofcontents

The key idea of `algebra` is to provide useful interfaces for basic cryptography math. It's based on NilFoundation fork of Boost.Multiprecision so that it can be used with boost cpp\_int, gmp or other backends.

We expanded Boost.Multiprecision with `modular_adaptor`, which is actually a multi-precision number by some modular. It contains modular number-specific algorithms using Montgomery representation. It also supports compile-time computations, because it gives us opportunity to implement algebra constructions as constexpr.

For crypto3 we needed to use field and curve arithmetic in compile time, which became possible thanks to compile-time `modular_adaptor`.

Algebra library consists of several modules listed below:

1. Fields arithmetic
2. Elliptic curves arithmetic
3. Pairings on elliptic curves
4. Multi-exponentiation algorithm (will be part of some other module after a while)
5. Matrices and vectors

### Fields Architecture ## <a href="#fields_architecture" id="fields_architecture"></a>

Fields are a wrapper over `multiprecision` module and concept of `modular_adaptor` number. So it basically consist of several parts listed below:

1. Field Policies
2. Field Extensions (e.g. Fp2, Fp4)
3. Field Parameters
4. Field Element Algorithms, which are actually wrappers over the `multiprecision` operations.

@dot digraph fields\_arch { bgcolor="#151515" rankdir="TB" node \[shape="box"]

a \[label="Field Policies" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica" URL="@ref field\_policies"]; b \[label="Field Extensions" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica" URL="@ref field\_extensions"]; c \[label="Field Parameters" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica" URL="@ref field\_parameters"]; d \[label="Field Element Algorithms" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica" URL="@ref field\_element\_algorithms"];

a -> b; b -> c; c -> d; } @enddot

#### Field Policies ### <a href="#field_policies" id="field_policies"></a>

A field policy describes its essential parameters such as `modulus`, `arity` or `mul_generator` - multiply generator.

#### Field Extensions ### <a href="#field_extensions" id="field_extensions"></a>

For the purposes of effective field/elliptic curve operations and pairings evaluation fields are arranged as a field tower.

For example, this is the tower used for `bn128` and `bls12_381` operations and pairings evaluation:

Fp -> Fp2 -> Fp6 -> Fp12;

@dot digraph fp12\_2over3over2\_arch { bgcolor="#151515" rankdir="TB" node \[shape="box"]

a \[label="Fp12" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; b \[label="Fp6" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; c \[label="Fp2" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; d \[label="Fp" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"];

a -> b; b -> c; c -> d; } @enddot

There are also the following towers implemented:

Fp -> Fp3 -> Fp6 -> Fp12;

@dot digraph fp12\_2over2over3\_arch { bgcolor="#151515" rankdir="TB" node \[shape="box"]

a \[label="Fp12" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; b \[label="Fp6" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; c \[label="Fp3" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; d \[label="Fp"color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"];

a -> b; b -> c; c -> d; } @enddot

Fp -> Fp2 -> Fp4 -> Fp12;

@dot digraph fp12\_3over2over2\_arch { bgcolor="#151515" rankdir="TB" node \[shape="box"]

a \[label="Fp12" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; b \[label="Fp4" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; c \[label="Fp2" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; d \[label="Fp" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"];

a -> b; b -> c; c -> d; } @enddot

#### Field Parameters ### <a href="#field_parameters" id="field_parameters"></a>

Other field parameters are kept in the specific structures. All this structures inherit from basic `params` structure, containing all the basic parameters.

For example, `extension_params` structure keeps all the parameters needed for field and field extensions arithmetical operation evaluations.

#### Field Element Algorithms ### <a href="#field_element_algorithms" id="field_element_algorithms"></a>

Field element corresponds an element of the field and has all the needed methods and overloaded arithmetic operators. The corresponding algorithms are also defined here. As the backend they use now Boost::multiprecision, but it can be easily changed.

### Elliptic Curves Architecture ## <a href="#curves_architecture" id="curves_architecture"></a>

Curves were build upon the `fields`. So it basically consist of several parts listed below:

1. Curve Policies
2. Curve g1, g2 group element arithmetic
3. Basic curve policies

@dot digraph curves\_arch { bgcolor="#151515" rankdir="TB" node \[shape="box"]

a \[label="Curve Policies" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica" URL="@ref curve\_policies"]; b \[label="Curve Element Algorithms" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica" URL="@ref curve\_element\_algorithms"]; c \[label="Basic curve policies" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica" URL="@ref basic\_curve\_policies"];

a -> b; b -> c; } @enddot

#### Curve Policies ### <a href="#curve_policies" id="curve_policies"></a>

A curve policy describes its parameters such as base field modulus `p`, scalar field modulus `q`, group element types `g1_type` and `g2_type`. It also contains `pairing_policy` type, needed for comfortable usage of curve pairing.

#### Curve Element Algorithms ### <a href="#curve_element_algorithms" id="curve_element_algorithms"></a>

Curve element corresponds an point of the curve and has all the needed methods and overloaded arithmetic operators. The corresponding algorithms are based on the underlying field algorithms are also defined here.

#### Basic Curve Policies ### <a href="#basic_curve_policies" id="basic_curve_policies"></a>

Main reason for existence of basic policy is is that we need some of it params using in group element and pairing arithmetic. So it contains such parameters that are needed by group element arithmetic e.g. coeffs `a` and `b` or generator coordinates `x`, `y`. It also contains all needed information about the underlying fields.

### Pairing Architecture ## <a href="#pairing_architecture" id="pairing_architecture"></a>

Pairing module consist of some internal functions and frontend interface templated by Elliptic Curve.
