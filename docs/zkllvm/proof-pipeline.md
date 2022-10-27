---
description: Paceholder proof pipeline
---

# Proof Pipeline

## Introduction

crypto3 library provides  all primitives for developers to write zero knowledge circuits  and provides API's to generate , verify proof. The circuits can be written for any zk scheme ex: SNARKS : plonk , .... or STARKS : ..... (TODO).

This guide does not explain the details of how to write circuts. It assumes the circuits are already written and available to the user via =nil; proof market.  This guide explains the pipelines for how proof generators and verifyers can use the circits and the artifacts.



### Step 1: Accessing the circuit

Circuits are constraints which are usually presented to the user in a seralised format. The first step the prover needs to do is to access this circuit and des-eralise it via API's provided in the crypto3 suite.

### Step 2: Generating the execution trace

Once the circuit is deserialied , it is ready to be instantiated. The instantiation of a circuit takes public parameters and witness parameters (private data) and this generates an execution trace (assignment table).

```shell
//pseudocode
zkinterpreter circuit.bytecode -i run_time_input.json -o circuit.bin -e assignment_table.bin 
```



### Step 3: Pre-processing data

Once the assignment table is generated , the prover is required to pre-process some data out of the execution trace. This step optimises the time required for verification.

```
// Some code
```

### Step 4: Generating Proof & Marshalling

The next step is to generate the proof via the API in the SDK. The proof once generated can be marshalled along with the pre-processed data from step 2. Marshalling allows for transferring proof between two entities

```cpp
// Pseudocode
main(){
   zk::plonk::circuit circuit_instance= marshalling::pack("circuit.bin");
   zk::plonk::assignment assignment_instance= marshalling::pack("assignment_table.bin");
   placeholder_params = ...;
   preprocessed_data = placeholder::preprocess(circuit_instance, assignment_instance);
   proof = placeholder::prove(circuit_instance, assignment_instance, preprocessed_data);
   std::cout << marshalling::pack(proof);
} 
```



### Step 5: Verifying

The last step is verifying the proof. The verifier can acces the marshalled blob and de-seralise the data. Once the proof and the pre-processed data is obtained , the user can check the validity of the proof.

```cpp
// Pseudcode
main(){
   std:cin >> proof_bin;
   std::cin >> pub_preprocessed_data.bin;
   proof  = marshalling::pack(proof_bin);
   pub_preprocessed_data  = marshalling::pack(pub_preprocessed_data.bin)
   placeholder_params = ...;
   bool verification_result = placeholder::verifier(prrof, pub_preprocessed_data);
   assert(   verification_result);
}
```

