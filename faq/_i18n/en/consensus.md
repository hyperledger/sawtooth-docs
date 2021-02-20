# Sawtooth FAQ: Consensus Algorithms (including PoET)
  
<!--
  Copyright (c) 2018, Intel Corporation.
  © Copyright 2020, Dr Kent G LAU, <kenty@kenty.com>.
  Licensed under Creative Commons Attribution 4.0 International License
  https://creativecommons.org/licenses/by/4.0/
-->

**Contents**

  - [What consensus algorithms does Sawtooth support?](#what-consensus-algorithms-does-sawtooth-support)
    - [Devmode (name \"Devmode\", version 0.1)](#devmode-name-devmode-version-01)
    - [PBFT (name \"pbft\", version 0.1)](#pbft-name-pbft-version-01)
    - [PoET CFT (name \"PoET\", version 0.1)](#poet-cft-name-poet-version-01)
    - [PoET SGX (name \"PoET\", version 0.1)](#poet-sgx-name-poet-version-01)
    - [Raft (name \"raft\", version 0.1)](#raft-name-raft-version-01)
  - [Will Sawtooth support more consensus algorithms in the future?](#will-sawtooth-support-more-consensus-algorithms-in-the-future)
  - [Where is Raft documented?](#where-is-raft-documented)
  - [Does the PBFT implementation follow the original paper?](#does-the-pbft-implementation-follow-the-original-paper)
  - [Does the PoET CFT implement the same consensus algorithm as PoET SGX?](#does-the-poet-cft-implement-the-same-consensus-algorithm-as-poet-sgx)
  - [For PoET CFT (PoET Simulator), should I generate my own `simulator_rk_pub.pem` file or do I use the one in `/etc/sawtooth/` ?](#for-poet-cft-poet-simulator-should-i-generate-my-own-simulator_rk_pubpem-file-or-do-i-use-the-one-in-etcsawtooth-)
  - [What is unpluggable consensus?](#what-is-unpluggable-consensus)
  - [Can my Sawtooth network have validators with a mixture of PoET SGX and PoET CFT?](#can-my-sawtooth-network-have-validators-with-a-mixture-of-poet-sgx-and-poet-cft)
  - [What protections does PoET CFT have, since it is not BFT?](#what-protections-does-poet-cft-have-since-it-is-not-bft)
  - [What cloud services offer Intel SGX?](#what-cloud-services-offer-intel-sgx)
  - [Does PoET SGX function with Intel SGX on cloud services?](#does-poet-sgx-function-with-intel-sgx-on-cloud-services)
  - [I get this error during PoET SGX registration: \"Machine requires update (probably BIOS) for Intel SGX compliance.\"](#i-get-this-error-during-poet-sgx-registration-machine-requires-update-probably-bios-for-intel-sgx-compliance)
  - [Does Sawtooth require a certain processor to be deployed on a network?](#does-sawtooth-require-a-certain-processor-to-be-deployed-on-a-network)
  - [Does Sawtooth require Intel SGX?](#does-sawtooth-require-intel-sgx)
  - [How is PoET duration time computed?](#how-is-poet-duration-time-computed)
  - [Why does PoET use exponentially-distributed random variable function instead of a uniform function?](#why-does-poet-use-exponentially-distributed-random-variable-function-instead-of-a-uniform-function)
  - [Where is PoET 1.0 Specification?](#where-is-poet-10-specification)
  - [Why is PoET SGX Byzantine Fault Tolerant?](#why-is-poet-sgx-byzantine-fault-tolerant)
  - [Where is the PoET SGX Enclave configuration file?](#where-is-the-poet-sgx-enclave-configuration-file)
  - [I run `sudo -u sawtooth poet registration create . . .` and get `Permission denied: 'poet_genesis.batch'` error](#i-run-sudo--u-sawtooth-poet-registration-create -and-get-permission-denied-poet_genesisbatch-error)
  - [What does `Consensus not ready to build candidate block` mean?](#what-does-consensus-not-ready-to-build-candidate-block-mean)
  - [What does `Failed to create wait certificate: Cannot create wait certificate because timer has timed out` mean?](#what-does-failed-to-create-wait-certificate-cannot-create-wait-certificate-because-timer-has-timed-out-mean)
  - [How do I change the Sawtooth consensus algorithm?](#how-do-i-change-the-sawtooth-consensus-algorithm)
  - [How do I change the consensus algorithm for a network that has forked?](#how-do-i-change-the-consensus-algorithm-for-a-network-that-has-forked)
  - [Where can I find information on the proposed PoET2 algorithm?](#where-can-i-find-information-on-the-proposed-poet2-algorithm)
  - [What is the Intel Platform Developers Kit for Blockchain - Ubuntu?](#what-is-the-intel-platform-developers-kit-for-blockchain ubuntu)
  - [Where is the Consensus Engine API documented?](#where-is-the-consensus-engine-api-documented)
  - [What are the minimum number of nodes needed for PoET?](#what-are-the-minimum-number-of-nodes-needed-for-poet)
  - [Can PoET be configured for small networks?](#can-poet-be-configured-for-small-networks)
  - [How should peer nodes be distributed?](#how-should-peer-nodes-be-distributed)
  - [Can I restrict what validator nodes win consensus?](#can-i-restrict-what-validator-nodes-win-consensus)
  - [How do I restart a consensus engine?](#how-do-i-restart-a-consensus-engine)
  - [Do I start the consensus engine before or after the validator?](#do-i-start-the-consensus-engine-before-or-after-the-validator)
  - [What can cause a fork with PoET?](#what-can-cause-a-fork-with-poet)
  - [How do I fix PoET using Intel SGX IAS API version 2, which is end of life?](#how-do-i-fix-poet-using-intel-sgx-ias-api-version-2-which-is-end-of-life)

 

## What consensus algorithms does Sawtooth support?

### Devmode (name \"Devmode\", version 0.1)

* Only suitable for testing TPs with single validator deployments.
    Uses a simplified random-leader algorithm for development and
    testing. Not for production use

### PBFT (name \"pbft\", version 0.1)

* Leader-based, non-forking consensus algorithm with finality that
    provides Byzantine Fault Tolerance (BFT). Ideal for smaller,
    consortium-style networks that do not require open membership.

### PoET CFT (name \"PoET\", version 0.1)

* Also known as PoET Simulator. PoET with a simulated Intel SGX
    environment. Provides CFT similar to some other blockchains.
    Requires poet-validator-registry TP. Runs on any processor (does not
    require Intel or Intel SGX). Has Crash Fault Tolerance (CFT), but is
    not Byzantine Fault Tolerant (BFT)

### PoET SGX (name \"PoET\", version 0.1)

* Takes advantage of Intel SGX in order to provide consensus with
    Byzantine Fault Tolerance (BFT), like PoW algorithms have, but at
    very low CPU usage. PoET SGX is the only algorithm that has hardware
    requirements (a processor supporting SGX). Currently supported in
    Sawtooth 1.0 only.

### Raft (name \"raft\", version 0.1)

* Consensus algorithm that elects a leader for a term of arbitrary
    time. Leader replaced if it times-out. Raft is faster than PoET, but
    is CFT, not BFT. Also Raft does not fork. For Sawtooth Raft is new
    and still being stabilized.

 

## Will Sawtooth support more consensus algorithms in the future?

* Yes. With pluggable consensus, the idea is to have a meaningful set of
consensus algorithms so the \"best fit\" can be applied to an
application\'s use case. Raft is a recent addition\--still being
stabilized. Others are being planned.

* REMME.io has independently implemented Algorand Byzantine Agreement on
Sawtooth.

 

## Where is Raft documented?

* <https://sawtooth.hyperledger.org/docs/raft/nightly/master/> 
* To use,
basically set `sawtooth.consensus.algorithm` to `raft` and
`sawtooth.consensus.raft.peers` to a list of peer nodes (network public
keys).

 

## Does the PBFT implementation follow the original paper?

* Yes, it follows the original 1999 Castro and Liskov paper with some
modifications and optimizations.

 

## Does the PoET CFT implement the same consensus algorithm as PoET SGX?

* Yes, they are same same consensus algorithm. The difference is the PoET
CFT also simulates the enclave module, allowing PoET to run on non-Intel
SGX hardware.

 

## For PoET CFT (PoET Simulator), should I generate my own `simulator_rk_pub.pem` file or do I use the one in `/etc/sawtooth/` ?

* No, you use the one that is installed. It must match the private key
that is in the PoET Simulator. The public key is needed to verify
attestation verification reports from PoET.

 

## What is unpluggable consensus?

* Sawtooth supports unpluggable consensus, meaning you can change the
consensus algorithm on the fly, at a block boundary. Changing consensus
on the fly means it is done without stopping validators, flushing state,
or starting over with a new genesis block. It is also called Dynamic
Consensus.

 

## Can my Sawtooth network have validators with a mixture of PoET SGX and PoET CFT?

* No. You need to pick one consensus for all nodes. But you can change
consensus after the Sawtooth network has started.

 

## What protections does PoET CFT have, since it is not BFT?

* It is for systems that do not have Intel SGX and do not require BFT.
Both PoET CFT and PoET SGX have tests to guard against bad actors, such
as the \"Z Test\" to check a validator is not winning too frequently.
PoET CFT simulates the Intel SGX environment and provides CFT. That
said, PoET SGX is preferred because of the additional Intel SGX
protections for generating the wait time.

 

## What cloud services offer Intel SGX?

* Intel SGX is available on IBM cloud and Alibaba. Microsoft Azure offers
Intel SGX on their Azure Confidential Computing (ACC) platform.

 

## Does PoET SGX function with Intel SGX on cloud services?

* No. For PoET SGX to function, one also needs Platform Services (PSW),
which is not available from any cloud provider. Instead, one can use
PoET CFT, which is also supported. But other software software that
requires Intel SGX may be deployed on cloud services.

 

## I get this error during PoET SGX registration: \"Machine requires update (probably BIOS) for Intel SGX compliance.\"

* During EPID provisioning your computer is trying to get an anonymous
credential from Intel. If that process is failing one possibility is
that there\'s a network issue like a proxy. A second possibility is that
there\'s some firmware out of date and so the protocol isn\'t doing what
the backend expects it to. You can check for a firmware / BIOS update
for that platform.

* Intel SGX also needs to be enabled in the BIOS menu.

 

## Does Sawtooth require a certain processor to be deployed on a network?

* No. If you use PoET SGX consensus you need a processor that supports
SGX.

 

## Does Sawtooth require Intel SGX?

* No. Intel SGX is only needed if you use the hardened version of PoET,
PoET SGX. We also have a version of PoET that just uses conventional
software, PoET CFT, which runs on a Sawtooth network with any processor.

## How is PoET duration time computed?

* It is `duration = random_float(0,1) * local_mean_wait_time`

 

## Why does PoET use exponentially-distributed random variable function instead of a uniform function?

* That is to minimize the number of \"collisions\" in the distribution of
a given round of wait timers generated by the population, where
\"collision\" means two or more timers that are near the minimum of the
distribution and within some latency threshold. The distribution of the
random function is shaped by a population estimate of the network, which
is determined by examining the last N blocks. In an ideal world, you
want a distribution where one and only one random wait time is around
the desired inter block duration, and then there is a decent sized gap.

 

## Where is PoET 1.0 Specification?

* <https://sawtooth.hyperledger.org/docs/core/releases/latest/architecture/poet.html>

 

## Why is PoET SGX Byzantine Fault Tolerant?

* Because the PoET waiting time is enforced with an Intel SGX enclave.
There is also more defense-in-depth checks, but that doesn\'t make it
BFT. What makes it BFT is the wait time enforcement with Intel SGX. In
comparison, Bitcoin\'s PoW accomplishes the same thing with repeatedly
hashing, which is effectively the same thing (although more wasteful)
than PoET\'s trusted timer. For details, see the PoET 1.0 spec in the
link above.

 

## Where is the PoET SGX Enclave configuration file?

* It is at `/etc/sawtooth/poet_enclave_sgx.toml` . It is only for
configuring PoET SGX Enclave, not the PoET CFT (PoET without Intel SGX).
* A sample file is at
<https://github.com/hyperledger/sawtooth-poet/blob/master/sgx/packaging/poet_enclave_sgx.toml.example>
* The configuration is documented at
<https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/configuring_sawtooth/poet_sgx_enclave_configuration_file.html>

 

## I run `sudo -u sawtooth poet registration create . . .` and get `Permission denied: 'poet_genesis.batch'` error

* Change to a sawtooth user-writable directory before running the command
and make sure file `poet\_genesis.batch` does not already
exist:  
`cd /tmp; ls poet_genesis.batch`

 

## What does `Consensus not ready to build candidate block` mean?

* This message is usually an innocuous information message. It usually
means that the validator isn\'t yet registered in the validator registry
or that its previous registration has expired and it\'s waiting for the
new one to commit. The message occurs after the block publisher polls
the consensus interface asking if it is time to build the block. If not
enough time has elapsed, it logs that message.

* However, if that message is rampant in the logs on all but one node,
that might mean that none of them can register (they are deadlocked when
launching a network). There\'s a few things that can cause that.

* Unlikely but worth mentioning: are you mapping volumes into the
containers? If all the validators are trying to use the same data file
that would be bad. That would not happen unless all the nodes are on the
same host.

* More commonly, the defense-in-depth checks are too stringent during the
initial launch. You can relax these parameters (see
[Settings](/faq/settings/) in this FAQ) or, easier yet, relaunch the
network.

 

## What does `Failed to create wait certificate: Cannot create wait certificate because timer has timed out` mean?

* It means too much time has elapsed between the creation of the wait
timer and the attempt to finalize the block and create the wait
certificate. Look at the logs for that node and determine when it
started to publish the block prior to that error, and see what
transpired in between. When the timer expires, the validator is supposed
to wrap up the schedule immediately and create the block, so that
message is kind of unusual. In versions of Sawtooth before 1.0, we
waited until the entire schedule executed, which could be quite long
running, and this message was quite common.

 

## How do I change the Sawtooth consensus algorithm?

-   Install the software package containing the consensus engine you
    wish to use on all nodes, if it is not already installed.
-   Start any consensus-required TPs, if any, on all nodes (for example
    PoET requires the `sawtooth_validator_registry` TP).
-   Use the `sawset proposal create` subcommand to modify
    `sawtooth.consensus.algorithm` (along with any consensus-required
    settings). For an example, see
    <https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html>

* The initial default consensus algorithm is `devmode`, which is not for
production use.

* Here is an example that changes the consensus to Raft:

   `sawset proposal create --url http://localhost:8008 --key /etc/sawtooth/keys/validator.priv  \ sawtooth.consensus.algorithm=raft sawtooth.consensus.raft.peers=\ '["0276f8fed116837eb7646f800e2dad6d13ad707055923e49df08f47a963547b631", \ "035d8d519a200cdb8085c62d6fb9f2678cf71cbde738101d61c4c8c2e9f2919aa"]'`

 

## How do I change the consensus algorithm for a network that has forked?

* Bring the network down to one node with the preferred blocks and submit
your consensus change proposal. Bring in the other nodes, with any
consensus-required TPs running (for example, PoET requires the Validator
Registry TP).

 

## Where can I find information on the proposed PoET2 algorithm?

* PoET2 is different from PoET in that it supports Intel SGX without
relying on Intel Platform Services Enclave (PSE), making it suitable in
cloud environments. PoET2 no longer saves anything across reboots (such
as the clock, monotonic counters, or a saved ECDSA keypair). The PoET2
SGX enclave still generates a signed, random duration value. More
details and changes are documented in the PoET2 RFC at
<https://github.com/hyperledger/sawtooth-rfcs/pull/20/files> A video
presentation (2018-08-23) is at
<https://drive.google.com/drive/folders/0B_NJV6eJXAA1VnFUakRzaG1raXc>
(starting at 7:45)

 

## What is the Intel Platform Developers Kit for Blockchain - Ubuntu?

* The PDK is a small form factor computer with Intel SGX with Ubuntu,
Hyperledger Sawtooth, and development software pre-installed. For
information, see
<https://designintools.intel.com/Intel_Platform_Developers_Kit_for_Blockchain_p/q6uidcbkcpdk.htm>

 

## Where is the Consensus Engine API documented?

* At <https://github.com/hyperledger/sawtooth-rfcs/pull/4> 
* See also the
\"Sawtooth Consensus Engines\" video at
20180426-sawtooth-tech-forum.mp4, starting at 10:00, in directory
<https://drive.google.com/drive/folders/0B_NJV6eJXAA1VnFUakRzaG1raXc>

 

## What are the minimum number of nodes needed for PoET?

* PoET needs at least 3 nodes, but works best with at least 5 nodes. This
is to avoid Z Test failures (a node winning too frequently). In
production, to keep a blockchain safe, more nodes are always better,
regardless of the consensus. 10 nodes are good for internal testing. For
production, have 2 nodes per identity.

 

## Can PoET be configured for small networks?

* Yes, for development purposes. For production purposes, consider using
another consensus algorithm. For example, Raft or PBFT handles a small
number of nodes nicely. We recommend PBFT for small networks. Raft is
less interesting being CFT and not BFT, and having overall less testing.

* For PoET in a small blockchain network, disable defense-in-depth tests
for small test networks (say, \< \~12 nodes) with:

    `sawtooth.poet.block_claim_delay=1
    sawtooth.poet.key_block_claim_limit= 100000
    sawtooth.poet.ztest_minimum_win_count=999999999`

 

## How should peer nodes be distributed?

* Blockchain achieves fault tolerance by having its state (data)
completely duplicated among the peer nodes. Best practice means
distributing your nodes\--geographically and organizationally.
Distributing nodes on virtual machines sharing the same host does
nothing to guard against hardware faults. Distributing nodes at the same
site does not protect against site outages.

 

## Can I restrict what validator nodes win consensus?

* No. Every peer node validates blocks and every peer node can publish a
block. You can write your own plugin consensus module to restrict what
peer nodes win. Or modify an existing consensus module to experiment.

## How do I restart a consensus engine?

* First stop the validator, then restart the consensus engine. If you
leave the validator engine running, it will not connect to the restarted
consensus engine. See
<https://jira.hyperledger.org/projects/STL/issues/STL-1465>

 

## Do I start the consensus engine before or after the validator?

* The consensus engine can start before or after the validator. The
preferred order is to start the validator first, then the consensus
engine. If you start the consensus engine before the validator, the
consensus engine will retry connecting to the validator (through TCP
port 5050) until it the consensus engine is successful.

 

## What can cause a fork with PoET?

* In PoET, forks occur due to a network partition, the size of the
network, the time it takes to transfer and validate blocks across the
network, and the likelihood that two or more validator will think they
have "won" and therefore publish a block during this time period.

* TPs don't really affect forks, unless they have a severe impact on the
validation duration of the block. However, unresolvable forks due to
non-determinism, are likely a TP problem.

 

## How do I fix PoET using Intel SGX IAS API version 2, which is end of life?

* For those who are trying to use PoET SGX with IAS, you need to move to
the IAS API v3 interface. 
* Basically just go through and change the IAS
URLs from `/v2/` to `/v3/` The change needs to
be made in files
`ias\_client/sawtooth\_ias\_client/ias\_client.py` and
`ias\_client/tests/unit/test\_ias\_client.py` .

* The Intel SGX IAS v3 API is at
<https://software.intel.com/sites/default/files/managed/7e/3b/ias-api-spec.pdf>