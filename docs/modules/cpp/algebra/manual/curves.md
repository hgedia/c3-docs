---
description: Crypto3.Algebra Elliptic curves
---

# Curves

## Elliptic curves

The following curves are implemented&#x20;

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

### Usage

Curves are defined under the namespace `nil::crypto3::algebra::curves` A curve can be instantiated as follows and by including the relevant header.

```
curves::bls12<377>
```

Curves encompass one or more `field` types definitions `typedef` which respect the curve specific constants and domain. Curves are generally used along with the [pubkey](https://github.com/NilFoundation/crypto3-pubkey) library which enables a user to create public/private keys and perform cryptographic operations.

## Field Type

`field` type is a generic type which is specialised for the above curves. This type can be extended to any curves and operators are overloaded to perform arithmetic over their domain. They are optimised for arithmetic in the finite field.

### Usage

A field can be instantiated as

```
field<254> //254 - is the Modulus Bits
```

Specialised for curves as base fields and scalar fields are usually defined as

```
//fields/secp/secp_k1
struct secp_k1_base_field<256> : public field<256>
struct secp_k1_scalar_field<256> : public field<256>
```

## Example

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

