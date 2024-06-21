
# Session 1 : 12 June 2024

High level session to explan what are SNARKS and give a whirlwind tour of the design landsacpe.

ref: chapters 1 and 19 from the book

## What is a SNARK (Succint Non Interactive Argument of Knolwedge)

Lets an untrusted prover prove to anyone that it knows some data satisfying something. We call the data a witness.

Witness examples

- Prove that you know a secret key that matches a public key (or you know a message the results in a specified hash) - security use
- Prove a set of digitial signatures have authorized a set of transactions (a block chain rollup) - Succint so we save processing for the system (only the prover has to process, all the nodes just verify)
- Prove that you have executed (have an execution trace) a program on a VM - Generic usage to prove that some computation has been done (on a ZK Virtual Machine (zkVM)) (VC - verifiable computing)

A SNARK is ZK if the prover reveals nothing about the witness except that it satisfies the claimed proprety. - need ZK SNARK

Moving on to looking at the design space of SNARKS or the SNARK toolchain. There is a general split into 1) frontend and 2) Backend. The Front end will take some kind of program or procedure that the developer has created to check the witness validity and turn it into something that the back end can prove and verify (an IR). The backend implments the prover and verifier parts on the front end.

## Frontend

For example in the case of proving that you know a secret key the Developer will design a witness checking program (e.g. a rust program) that checks that a key when hashed results in the expected result. The front end will turn that program into some thing that can be proved (IR).

### Intermediate Represenmtation (IR)

Examples of IRs are circuits or constraint systems : [R1CS](https://learn.0xparc.org/materials/circom/additional-learning-resources/r1cs%20explainer/), [AIR](https://aszepieniec.github.io/stark-anatomy/stark), [Plonkish](https://zcash.github.io/halo2/concepts/arithmetization.html).

### Front end languages

There are now many ways (i.e. language). Trend to using higher level languages to make things easier for the developer use standard industry language tooling and push the security burden to the VM (as opposed to custom circuits where developers have to know what they are doing).

#### Very low level (circuit level)

- [Bellman](https://github.com/zkcrypto/bellman)
- [Circom](https://iden3.io/circom)
- [halo2](https://zcash.github.io/halo2/index.html)

#### Domain Specific Language (DSL)

- [zokrates](https://github.com/Zokrates/ZoKrates)
- [xjsnark](https://github.com/akosba/xjsnark)
- [cairo](https://www.cairo-lang.org/)
- [o1js](https://docs.minaprotocol.com/zkapps/o1js)
- [noir](https://noir-lang.org/)

#### High Level (and VM)

- Rust, Java etc .. using standard tool chain to a zkVM
  -  [risc0](https://www.risczero.com/zkvm)
  -  [SP1](https://succinct.xyz/)
  -  [jolt](https://jolt.a16zcrypto.com/)

## Backend

Backend is the "actual" SNARK. Generally there are three steps

1) A polynomial IOP. [see intro in this newer paper](https://eprint.iacr.org/2020/1022.pdf)
2) A polynomial commitement scheme [PCS](https://erroldrummond.gitbook.io/snark-fundamentals/part-2/polynomial-commitment-schemes)
3) Apply fiat shamir (turns an interactive public coin protocal into a non interactive scheme)

Add in zero knowledge later. There are some specific techniques. e.g. !) Run through a zkSNARK prove that you know the proof. 2) Prover sends hiding commmitments instead of field elements.

Also see [here](https://medium.com/@Luca_Franceschini/a-guide-to-zero-knowledge-proofs-part-2-7904dee9758d)

Groth16 is an exception to this design paradigm (uses a linear PCP (), does use polynomial commitement (KZG), does not use fiat shamir (trusted setup covers))

### Polynomial IOP (PIOP)

An interacitve protocoal where P (prover) sends large polynomials to the verifier (e.g. millions or billions of co-efficients (related to circuit size)). Verifier V will evaluate at random points during the proof. Why polynomials ? They have good distance amplification properties ... so two polynomials will dissagree at most points.

### Polynomial commitment schemen

Is what you need to avoid sending the whole polynomial to teh Verifier. Instead send a committment and the scheme lets teh verifiire open consitently at random points

### Concrete Examples

PIOPs and Polynomial Commitment can be mixed and matched except each can be univariate or mulitlinear only (some or both) and have to match.

#### PIOP three Categories

##### Constant Round (all are univariate)

- [Plonk](https://eprint.iacr.org/2019/953)
- [Marlin](https://eprint.iacr.org/2019/1047.pdf)
- STARK PIOP

#### MIPS (multi prover interactive proofs)

- Spartan

#### interactive proofs

- GKR

### Polynomial Commitement schemens (three categories)

#### trusted setup use EC pairing freindly groups (low verifier costs)

- [KZK](https://link.springer.com/chapter/10.1007/978-3-642-17373-8_11)

### transparent EC curves

- (Bulletproofs)[https://eprint.iacr.org/2017/1066] called also called (IPA)[https://eprint.iacr.org/2016/263]
- Dory
- Hyrax

### Hashing Based - Error correcting codes

- FRI
- Ligero
- Breakdown
- Binius
- FRI-Binius

## Other Design topics

Get best properties by mixing SNARKs using SNARK *composition*.

Acuumulation / folding schemes like e.g. nova, protostar useful for controlligg prover space.

Today there is a need chop a proof into small pieces and aggregate due to large hardware size needs.  instead of just doing recursive snarks use this accumulation or folding scheme.

## Next session

Start with more about prover and verifier cost trade offs and move into chap 1 of the book


## Reference

- [Session with Notes](https://youtu.be/QVNsxryPgUo?si=W3EqkjK2--S7ulIL)
- [Session without Notes](https://youtu.be/qQ7yIEJKCtE?si=AMlc5zYGi3Pzx6hb)
- [Notes](Session1Note.pdf)


```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```