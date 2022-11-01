---
description: Crypto3.Algebra Elliptic curves
---

# Elliptic Curves

The following elliptic curves are implemented&#x20;

* Barreto-Naehrig
* Babyjubjub
* BLS12 variants
* Brainpool
* [Curve25519](https://datatracker.ietf.org/doc/html/rfc7748#section-4.1)
* ed25519
* Edwards
* FRP?? TODO
* GOST
* Jubjub
* mnt4/mnt6
* NIST P-192/P-224/P-384/P-521
* [Pallas](https://zips.z.cash/protocol/protocol.pdf#pallasandvesta)
* secpk1
* secpr1
* secpv1
* sm2p
* vesta
* X9.62

## Usage

Curves are defined under the namespace `nil::crypto3::algebra::curves`&#x20;

A curve type is generally passed as a parameter to a cryptographic scheme (TODO : Link).  The template type can be instantiated as follows:

```cpp
using curve_bls_381 = curves::bls12_381; // As an existing typedef 
using curve_bls_377 = curves::bls12<377> // Explicityly passing variant
```

Curves encompass one or more `field` types definitions (via `typedef` )which respect the curve specific constants and domain. Curves are generally used along with the [pubkey](https://github.com/NilFoundation/crypto3-pubkey) library which enables a user to create public/private keys and perform cryptographic operations.

## Example

The class below describes a BLS curve which accepts a template parameter for variants. &#x20;

```cpp
template<std::size_t Version>
class bls12 {

	typedef detail::bls12_types<Version> policy_type;

public:
	typedef typename policy_type::base_field_type base_field_type;
	typedef typename policy_type::scalar_field_type scalar_field_type;

	template<typename Coordinates = coordinates::jacobian_with_a4_0,
			 typename Form = forms::short_weierstrass>
	using g1_type = typename detail::bls12_g1<Version, Form, Coordinates>;

	template<typename Coordinates = coordinates::jacobian_with_a4_0,
			 typename Form = forms::short_weierstrass>
	using g2_type = typename detail::bls12_g2<Version, Form, Coordinates>;

	constexpr static const bool has_affine_pairing = false;

	typedef typename policy_type::gt_field_type gt_type;
};

typedef bls12<381> bls12_381;
typedef bls12<377> bls12_377;

```

We define a number of types which will associate the class with the domain & field. The important types are :&#x20;

* `base_field_type :`  TODO
* `scalar_field_type :` TODO

