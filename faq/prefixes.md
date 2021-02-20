# FAQ Appendix: Transaction Family Prefixes 

<!--
  Copyright (c) 2018, Intel Corporation.
  © Copyright 2020, Dr Kent G LAU, <kenty@kenty.com>.
  Licensed under Creative Commons Attribution 4.0 International License
  https://creativecommons.org/licenses/by/4.0/
-->

This is an unofficial list of some Transaction Family (TF) prefixes.  There is no central registry, most or all of these TFs are found on GitHub ( <https://github.com/> especially
<https://github.com/hyperledger> and <https://github.com/hyperledger-labs> ).

Sawtooth addresses are 70 hex characters. The prefix is either the first 6 characters of the SHA-512 hash of the namespace, or, for some base namespaces, a \"hex word\". The Sawtooth Validator registry is an outlier. It uses the SHA-256 hash (not SHA-512) and hashes \"validator_registry\" (not "sawtooth_validator_registry\"). The remainder of the address is TF-specific and defined for each TF. Listing of a TF does not imply endorsement.

All data payloads are encoded in base64 after serializing. Sawtooth headers are serialized with Protobuf.

For base TF specifications, see
<https://sawtooth.hyperledger.org/docs/core/releases/latest/transaction_family_specifications/>

|TRANSACTION FAMILY NAME   |SERIALIZATION   |PREFIX   |PREFIX ENCODING   |
|---|---|---|---|
|settings   |Protobuf   |000000   |Validator settings.  Only required TF   |
|identity   |Protobuf   |00001d   |Validator Identity for TP/Validator keys   |
|sawtooth_validator_registry   |Protobuf   |6a4372   |PoET Validator Registry. Used by PoET consensus to track other validators. See note above about hash prefix .   |
|blockinfo   |Protobuf   |00b10c   |Validator Block Info. Used for SETH 00b10c00 metadata namespace info about other namespaces 00b10c01 block info namespace historic block info 00b10c0100....00<block # in hex> info on block at block #   |
|sabre   |Protobuf   |00ec00<br/>00ec01<br/>00ec02   |WebAssembly VM: NamespaceRegistry<br/>Wasm: ContractRegistry<br/>Wasm: Contracts   |
|seth   |Protobuf   |a68b06   |SETH (Sawtooth Ethereum VM)   |
|pdo_contract_instance_registry   |Protobuf   |aa2a93   |Private Data Objects (PDO) Contract Instance Registry   |
|pdo_ contract_ enclave_ registry	   |Protobuf   | 0b936f  |Private Data Objects (PDO) Contract Enclave Registry|
|ccl_ contract_ contract_ state_ registry	   |Protobuf   |db13a2   |Private Data Objects (PDO) Coordination and Commit Log (CCL) Contract State Registry
|
|**SOME EXAMPLE TFs**|   |   |   |
|battleship   |JSON   |6e10df   |Battleship example game   |
|intkey   |CBOR   |1cf126   |Integer Key. Full production example   |
|smallbank   |Protobuf   |332514   |Small Bank example app   |
|xo   |CSV-UTF8	   |5b7349   |Tic-tac-toe example game   |
|supply_chain   |Protobuf   |3400de   |Asset (Fish) Supply Chain example app   |
|marketplace   |Protobuf   |cd6744   |Marketplace example app   |
|transfer- chain	   |JSON-UTF8	   |19d832   |Simple Tuna Supply Chain app. Used for edX LFS171x class   |
|simplewallet   |CSV-UTF8	   |7e2664   |Simple Wallet minimal example   |
|cookiejar   |CSV-UTF8	   |a4d219   |Cookie Jar minimal example   |
|simple_ supply	   |Protobuf   |5d6af4   |Simple Supply example used for future edX LFS201 class   |
|pirate-talk	   |UTF8   |aaaaaa	   |Pirate Talk minimal example
   |
|**SOME THIRD-PARTY PRODUCTION TFs**|   |   |   |
|rbac   |Protobuf   |8563d0   |T-Mobile NEXT Identity Platform|
|sawtoothekyc   |Protobuf   |dbf420   |Primechain Blockchain-eKYC bank records   |
|pub_key   |Protobuf   |a23be1   |REMME REMChain   |
|bitagora- ballots	   |JSON   |b42861   |Bitagora voting ballot   |
|bitagora- polls	   |JSON   |154f9c   |Bitagora voting ballot   |