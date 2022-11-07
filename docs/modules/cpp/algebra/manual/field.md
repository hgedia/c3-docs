# field

## Field

`field` type is a generic type which is an extension of `boost::multiprecision`.&#x20;

All fields implemented in algebra library conform to the concept of a field type. A field must conform to the field traits defined in `algebra/include/nil/crypto3/algebra/type_traits.hpp`

Field is generally specialised per curve and it holds the domain and other curve specific constants. Field consists of a set of stateless policies and with the extension of `modular_adaptor` allows for finite field arithmetic. If you wish to use elliptic curve related arithmetic , see __ `element_fp`&#x20;

### Usage

Fields are defined under the namespace `nil::crypto3::algebra::fields` and header need to be included ex: `nil/crypto3/algebra/fields/<curve>/scalar_field.hpp`

A field can be instantiated as:

```cpp
field<254> //254 - is the modulus Bits
```

Specialised for curves as `base` fields and `scalar` fields are usually defined as

```cpp
//fields/secp/secp_k1
struct secp_k1_base_field<256> : public field<256>
struct secp_k1_scalar_field<256> : public field<256>
```

#### Example#1

Below we see a specialisation of a field type to a `bls12_base_field`.

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

#### Example#2&#x20;

In this example , we see finite field element arithmetic performed on filed elements over`BLS12-381` curve. We see elements are defined conform to `boost::multiprecision` literals defined [here](https://www.boost.org/doc/libs/1\_73\_0/libs/multiprecision/doc/html/boost\_multiprecision/tut/lits.html).

```cpp
#include <nil/crypto3/detail/literals.hpp>
#include <nil/crypto3/algebra/fields/bls12/base_field.hpp>

using namespace nil::crypto3::algebra;
using namespace nil::crypto3::multiprecision;

enum field_operation_test_elements : std::size_t {
    e1,
    e2,
    e1_plus_e2,
    e1_minus_e2,
    e1_mul_e2,
    e1_dbl,
    e2_inv,
    e1_pow_C1,
    e2_pow_2,
    e2_pow_2_sqrt,
    minus_e1,
    elements_set_size
};

enum field_operation_test_constants : std::size_t { C1, constants_set_size };
typedef std::size_t constant_type;

int main() {

    using policy_type = fields::bls12_fq<381>;
    using value_type = typename policy_type::value_type;
    using test_set_t = std::array<value_type, elements_set_size>;
    using const_set_t = std::array<constant_type, constants_set_size>;

    constexpr test_set_t elements1 = {
            0x3d9cb62ebac9d6c7b94245d2d6144d500f218bb90a16a1e4f70d98fd44b4b9ee274de15a0a3d231dac1eaa449d31404_cppui381,
            0x15c88779fc8a30cca95ec4bbf71aa4c302bccf7dc571e6e45fbf1ed24989ec23dff741ca00597f4ab1fc628304e8761b_cppui381,
            0x19a252dce836ce3924f2e919247be99803aee83956135102af2ff8621dd537c2c26c1fdfa0fd517c8cbe4d274ebb8a1f_cppui381,
            0x81255d328a2533a1d51075779924ce962ac94c2beb495f956e28d5e8172559f21299c4a519e52e6e2c4882144ea4894_cppui381,
            0x4e02d210a60d52212c21056e050b7f7b6aa45c2fb85e692b1fef9e3e6fb43b2bf8103105f43daca458e4dccc9f5236c_cppui381,
            0x7b396c5d7593ad8f72848ba5ac289aa01e431772142d43c9ee1b31fa896973dc4e9bc2b4147a463b583d54893a62808_cppui381,
            0x68241cb698160ee94897ec6600bc997de3fed563dfc36a758334c71dc76a2473571cfbc0f674038ee748add41e4277a_cppui381,
            0xbb4588b98237fefeba65f928e69da9106c690e02c70361947b39d0f5a6d462096431d375d4b66ae7e4daef9f2400a09_cppui381,
            0x2e7ebd9b39f65a9485b32b52269baa84b2d33a80c8747c994b1e58c0caa09b4acf7685583898549db1029a1de657d8a_cppui381,
            0x4388a703cf5b5cda1bce2fa4c31081461ba7c072e132bdb0771b3cead270a003eb4be34b0fa80b508029d7cfb173490_cppui381,
            0x162746874dd3492dcf87835915ea6802638532c962e3a8a117bff9112265aa853c3721e910b02dcddf3d155bb62c96a7_cppui381};
    constexpr const_set_t constants1 = {865433380};

    //Addition
    static_assert(elements1[e1] + elements1[e2] == elements1[e1_plus_e2], "add error");

    //Field Subtraction
    static_assert(elements1[e1] - elements1[e2] == elements1[e1_minus_e2], "sub error");

    //Multiplication
    static_assert(elements1[e1] * elements1[e2] == elements1[e1_mul_e2], "mul error");

    // Double
    static_assert(elements1[e1].doubled() == elements1[e1_dbl], "dbl error");

    //Inverse
    static_assert(elements1[e2].inversed() == elements1[e2_inv], "inv error");

    //exponentiation
    static_assert(elements1[e1].pow(constants1[C1]) == elements1[e1_pow_C1], "pow error");

    //Square
    static_assert(elements1[e2].squared() == elements1[e2_pow_2], "sqr error");

    //Square-root
    static_assert((elements1[e2].squared()).sqrt() == elements1[e2_pow_2_sqrt], "sqrt error");

    //Negative
    static_assert(-elements1[e1] == elements1[minus_e1], "neg error");

    return 0;
}
```

## Field Extensions

Following field extensions are already built in and are used in the suite.

* FP2
* FP3
* FP4
* FP6\_2OVER3
* FP6\_3OVER2
* FP12\_2OVER3OVER2

Each of the above define a type trait which is then exhibited by specialisations.

### Usage

#### Example#1

```cpp
template<typename BaseField>
struct fp2 {
	typedef BaseField base_field_type;
	typedef base_field_type policy_type;
	typedef detail::fp2_extension_params<policy_type> extension_policy;
	typedef typename extension_policy::underlying_field_type underlying_field_type;

	constexpr static const std::size_t modulus_bits = policy_type::modulus_bits;
	typedef typename policy_type::integral_type integral_type;

	typedef typename policy_type::extended_integral_type extended_integral_type;

	constexpr static const std::size_t number_bits = policy_type::number_bits;
	typedef typename policy_type::modular_type modular_type;
	typedef typename policy_type::modular_backend modular_backend;

	constexpr static const integral_type modulus = policy_type::modulus;

	typedef typename detail::element_fp2<extension_policy> value_type;

	constexpr static const std::size_t arity = 2;
	constexpr static const std::size_t value_bits = arity * modulus_bits;
};
```

In the above BLS base field example we can see an `element_fp` type used which adheres to traits of `fp`

```cpp
typedef typename detail::element_fp<params<bls12_base_field<381>>> value_type;
```

