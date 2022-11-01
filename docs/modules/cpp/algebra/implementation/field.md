# Field

## Field Architecture  <a href="#fields_architecture" id="fields_architecture"></a>

Fields are a wrapper over `multiprecision` module and concept of `modular_adaptor` number. So it basically consist of several parts listed below:

1. Field Policies
2. Field Extensions (e.g. Fp2, Fp4)
3. Field Parameters
4. Field Element Algorithms, which are actually wrappers over the `multiprecision` operations.

![](<../../../../.gitbook/assets/image (1).png>)

### Field Policies  <a href="#field_policies" id="field_policies"></a>

A field policy describes its essential parameters such as `modulus`, `arity` or `mul_generator` - multiply generator.

### Field Extensions <a href="#field_extensions" id="field_extensions"></a>

For the purposes of effective field/elliptic curve operations and pairings evaluation fields are arranged as a field tower.

For example, this is the tower used for `bn128` and `bls12_381` operations and pairings evaluation:

Fp -> Fp2 -> Fp6 -> Fp12;

![](<../../../../.gitbook/assets/image (4).png>)

There are also the following towers implemented:

Fp -> Fp3 -> Fp6 -> Fp12;

![](../../../../.gitbook/assets/image.png)

Fp -> Fp2 -> Fp4 -> Fp12;

@dot digraph fp12\_3over2over2\_arch { bgcolor="#151515" rankdir="TB" node \[shape="box"]

a \[label="Fp12" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; b \[label="Fp4" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; c \[label="Fp2" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"]; d \[label="Fp" color="#f5f2f1" fontcolor="#f5f2f1" fontname="helvetica"];

a -> b; b -> c; c -> d; } @enddot

### Field Parameters  <a href="#field_parameters" id="field_parameters"></a>

Other field parameters are kept in the specific structures. All this structures inherit from basic `params` structure, containing all the basic parameters.

For example, `extension_params` structure keeps all the parameters needed for field and field extensions arithmetical operation evaluations.

### Field Algorithms <a href="#field_element_algorithms" id="field_element_algorithms"></a>

Field element corresponds an element of the field and has all the needed methods and overloaded arithmetic operators. The corresponding algorithms are also defined here. As the backend they use now Boost::multiprecision, but it can be easily changed.

### &#x20;<a href="#curves_architecture" id="curves_architecture"></a>
