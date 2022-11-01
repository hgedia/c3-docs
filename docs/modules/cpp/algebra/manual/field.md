# Field

`field` type is a generic type which is an extension of `boost::multiprecision` . It is usually specialised for a particular curve and operators are overloaded to perform arithmetic over their domain. They are optimised for arithmetic in the finite field.

## Usage

A field can be instantiated as

```cpp
field<254> //254 - is the modulus Bits
```

Specialised for curves as base fields and scalar fields are usually defined as

```cpp
//fields/secp/secp_k1
struct secp_k1_base_field<256> : public field<256>
struct secp_k1_scalar_field<256> : public field<256>
```

## Example (TO REMOVE)

Below we see a specialisation of a field type to a bls12\_base\_field.

```cpp
template<std::size_t Version>
struct bls12_base_field;

template<>
struct bls12_base_field<381> : public field<381> {
	typedef field<381> policy_type;

	constexpr static const std::size_t modulus_bits = policy_type::modulus_bits;
	typedef typename policy_type::integral_type integral_type;

	typedef typename policy_type::extended_integral_type extended_integral_type;

	constexpr static const std::size_t number_bits = policy_type::number_bits;

	constexpr static const integral_type modulus =
		0x1A0111EA397FE69A4B1BA7B6434BACD764774B84F38512BF6730D2A0F6B0F6241EABFFFEB153FFFFB9FEFFFFFFFFAAAB_cppui381;

	typedef typename policy_type::modular_backend modular_backend;
	constexpr static const modular_params_type modulus_params = modulus;
	typedef nil::crypto3::multiprecision::number<
		nil::crypto3::multiprecision::backends::modular_adaptor<
			modular_backend,
			nil::crypto3::multiprecision::backends::modular_params_ct<modular_backend, modulus_params>>>
		modular_type;

	typedef typename detail::element_fp<params<bls12_base_field<381>>> value_type;

	constexpr static const std::size_t value_bits = modulus_bits;
	constexpr static const std::size_t arity = 1;
};
```
