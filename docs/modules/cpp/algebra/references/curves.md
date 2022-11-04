# curves

## curves <a href="#block_cipher_concept" id="block_cipher_concept"></a>

A `curve` is a policy intended to represent an elliptic curve of the form $$y^{2}=x^{3}+ax+b$$

#### Requirements

The type `X` satisfies `curve` if

Given

* `BaseFieldType`, the type named by `X::base_field_type`
* `ScalarFieldType`, the type named by `X::scalar_field_type`
* `GType`, the type named by `X::g1_type`

``

The following type members must be valid and have their specified effects

| Expression             | Type        | Requirements and Notes                     |
| ---------------------- | ----------- | ------------------------------------------ |
| `X::base_field_type`   | `FieldType` | `FieldType` type satisfies `field` concept |
| `X::scalar_field_type` | `FieldType` | `FieldType` type satisfies `field` concept |
| `X::`g1\_type          | `FieldType` | `FieldType` type satisfies `field` concept |



## curves group <a href="#block_cipher_concept" id="block_cipher_concept"></a>

TODO : Describe a curve group

#### Requirements <a href="#block_cipher_concept" id="block_cipher_concept"></a>

The type `X` satisfies `curve_group` if

Given

* `FieldType`, the type named by `X::`value\_type
* `FieldType`, the type named by `X::`value\_type
* `CurveType`, the type named by `X::curve_type`



The following static data member definitions must be valid and have their specified effects

| Expression       | Type          | Requirements and Notes                                |
| ---------------- | ------------- | ----------------------------------------------------- |
| `X::word_bits`   | `std::size_t` | `Integral` bits amount in `WordType`                  |
| `X::key_bits`    | `std::size_t` | `Integral` bits amount in `KeyType`                   |
| `X::block_bits`  | `std::size_t` | `Integral` bits amount in `BlockType`                 |
| `X::block_words` | `std::size_t` | `Integral` amount of `WordType` values in `BlockType` |
| `X::rounds`      | `std::size_t` | `Integral` amount of rounds the algorithm does.       |

The following expressions must be valid and have their specified effects

| Expression              | Requirements                                                                                                                                                                                                                 | Return Type   |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| `X(key_type)`           | Constructs stateful `BlockCipher` object with input key of `key_type`                                                                                                                                                        | `BlockCipher` |
| `X.encrypt(block_type)` | Encrypts a block of data in decoded format specified for particular algorithm. A block can be of a variable size. Should be a non-mutating function depending only on a `BlockCipher` object inner state of `key_type` type. | `block_type`  |
| `X.decrypt(block_type)` | Decrypts a block of data in encoded format specified for particular algorithm. A block can be of a variable size. Should be a non-mutating function depending only on a `BlockCipher` object inner state of `key_type` type. | `block_type`  |
