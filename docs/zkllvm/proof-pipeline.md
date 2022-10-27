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



### Step 3: Pre-processing data

Once the assignment table is generated , the prover is required to pre-process some data out of the execution trace. This step optimises the time required for verification.

### Step 4: Generating Proof & Marshalling

The next step is to generate the proof via the API in the SDK. The proof once generated can be marshalled along with the pre-processed data from step 2. Marshalling allows for transferring proof between two entities



### Step 5: Verifying

The last step is verifying the proof. The verifier can acces the marshalled blob and de-seralise the data. Once the proof and the pre-processed data is obtained , the user can check the validity of the proof.







1. Build the circuit (aka set of constraints) and the execution trace (priv+pub assignment table) by yourself or by zkllvm compiler- [https://github.com/NilFoundation/crypto3-blueprint/blob/master/test/test\_plonk\_component.hpp#L124](https://github.com/NilFoundation/crypto3-blueprint/blob/master/test/test\_plonk\_component.hpp#L124)
2. Preprocessing - [https://github.com/NilFoundation/crypto3-blueprint/blob/master/test/test\_plonk\_component.hpp#L151-L158](https://github.com/NilFoundation/crypto3-blueprint/blob/master/test/test\_plonk\_component.hpp#L151-L158)
3. Prove - [https://github.com/NilFoundation/crypto3-blueprint/blob/master/test/test\_plonk\_component.hpp#L179](https://github.com/NilFoundation/crypto3-blueprint/blob/master/test/test\_plonk\_component.hpp#L179)
4. Marshalling: {proof + pub preprocessed}->byte blob, send it to some storage
5. Download the byte blob, use marshalling: byte blob -> {proof + pub preprocessed}
6. Verify - [https://github.com/NilFoundation/crypto3-blueprint/blob/master/test/test\_plonk\_component.hpp#L182](https://github.com/NilFoundation/crypto3-blueprint/blob/master/test/test\_plonk\_component.hpp#L182)

zkllvm input.cpp -o circuit.bytecode - bytecode circuit IR without the execution trace (edited) [12:45](https://nilfoundation.slack.com/archives/D03QHPTDYAH/p1666784709553799)zkinterpreter circuit.bytecode -i run\_time\_input.json -o circuit.bin -e assignment\_table.bin - circuit and execution trace are generated (edited) [12:46](https://nilfoundation.slack.com/archives/D03QHPTDYAH/p1666784815288149)prover:main(){\
&#x20;  zk::plonk::circuit circuit\_instance= marshalling::pack("circuit.bin");\
&#x20;  zk::plonk::assignment assignment\_instance= marshalling::pack("assignment\_table.bin");   placeholder\_params = ...;   preprocessed\_data = placeholder::preprocess(circuit\_instance, assignment\_instance);   proof = placeholder::prove(circuit\_instance, assignment\_instance, preprocessed\_data);   std::cout << marshalling::pack(proof);\
} (edited)&#x20;





verifier: main(){ std:cin >> proof\_bin; std::cin >> pub\_preprocessed\_data.bin; proof = marshalling::pack(proof\_bin); pub\_preprocessed\_data = marshalling::pack(pub\_preprocessed\_data.bin) placeholder\_params = ...; bool verification\_result = placeholder::verifier(prrof, pub\_preprocessed\_data); assert( verification\_result); } (edited)







