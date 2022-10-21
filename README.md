# EIP-712 verification of Verifiable Credentials issued by https://github.com/spruceid/didkit

This is a simple example of how Verifiable Credentials can be verified on-chain.

It is based on the [EIP-712 standard for typed structured data hashing and signing ](https://eips.ethereum.org/EIPS/eip-712) and [Ethereum EIP712 Signature 2021](https://w3c-ccg.github.io/ethereum-eip712-signature-2021-spec/).


### Sign and Verify didkit credential using Javascript
`yarn ts-node utils/didkitSign.ts` this will sign and generate types for a Verifiable Credential using [DIDKit](https://github.com/spruceid/didkit)

### On chain verification
[DIDStampVCVerifier](contracts/DIDStampVCVerifier.sol) - On chain verification that the signer of the provided credential is the issuer. 
[Test](test/DIDStampVCVerifier.ts) - Verify that signed credential is valid and signed by issuer

### Gas usage of simple verification
<img width="840" alt="Screen Shot 2022-10-21 at 10 56 56 AM" src="https://user-images.githubusercontent.com/6887938/197249444-ef5084d3-98b8-4ba0-8a90-2598577bfa9c.png">


### Pre Requisites

Before running any command, you need to create a `.env` file and set a BIP-39 compatible mnemonic or a private key as an environment
variable. Follow the example in `.env.example`. If you don't already have a mnemonic, use this [website](https://iancoleman.io/bip39/) to generate one.

Then, proceed with installing dependencies:

```sh
$ yarn install
```

Populate `.env` file with at least `INFURA_API_KEY`

### Some remarks

- The proof object is based on [Ethereum EIP712 Signature 2021](https://w3c-ccg.github.io/ethereum-eip712-signature-2021-spec/).
- "@" is not a valid character in a variable name in Solidity, hence why the `@context` and `@type` properties are renamed `_context` and `_type` respectively.
- The `did:pkh` method is used which allows us to turn the Ethereum address of the VC issuer / signer into a valid DID. The inverse of this "process" is done on-chain by the pseudo-resolver in `DIDpkhAdapter.sol`.

<br/>

Then, the content of the whole VC (minus the proof object) and the signature attached to it is verified on-chain against the Ethereum address in `issuer`. This is done by calling `verifyStampVc()`. If the verification is successful you should see:

<br/>

## Next steps

- [ ] Create an example of a VC as voucher that can be redeemed against something such as ERC20 tokens or 721 tokens
- [ ] Generalize the implementation so that an on-chain VC verifier is not tightly coupled to a specific VC schema

<br/>
<br/>

## Generic Repo Usage

### Compile

Compile the smart contracts with Hardhat:

```sh
$ yarn compile
```

### TypeChain

Compile the smart contracts and generate TypeChain artifacts:

```sh
$ yarn typechain
```

### Lint Solidity

Lint the Solidity code:

```sh
$ yarn lint:sol
```

### Lint TypeScript

Lint the TypeScript code:

```sh
$ yarn lint:ts
```

### Test

Run the Mocha tests:

```sh
$ yarn test
```

### Coverage

Generate the code coverage report:

```sh
$ yarn coverage
```

### Report Gas

See the gas usage per unit test and average gas per method call:

```sh
$ REPORT_GAS=true yarn test
```

### Clean

Delete the smart contract artifacts, the coverage reports and the Hardhat cache:

```sh
$ yarn clean
```

## Syntax Highlighting

If you use VSCode, you can get Solidity syntax highlighting via the [vscode-solidity](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity) extension.



## Hugely influenced by https://github.com/ra-phael/eip712-vc 🙌
