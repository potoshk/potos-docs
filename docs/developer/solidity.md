# Solidity

This chapter outlines the fundamental concepts, development processes, and example code in Solidity. Solidity is already well documented on its official websites, so for any language details, please refer to [Solidity documentation](https://solidity.readthedocs.io/en/latest/index.html).

POTOS is officially compatible with [**Cancun**](https://ethereum.org/en/history/#dencun) EVM version, and is compatible to Solidity version 0.8.26.

## Develoment tools

Smart contract development for POTOS can be streamlined with tools like:

- [Remix](https://remix.ethereum.org/), a browser-based IDE. For more information about connecting Remix to POTOS, refer to the [Remix usage guide](./remix_usage.md).
- [Hardhat](https://hardhat.org/), a comprehensive development framework. For more information about deploying smart contracts using Hardhat, refer to the [Hardhat usage guide](./hardhat_usage.md).

## Solidity Compiler Installation

It is convenient to utilize Remix or Hardhat to develop smart contracts, so we highly recommend using these tools. However, for local Solidity compiler usage, follow the instructions on [Installing the Solidity Compiler](https://docs.soliditylang.org/en/latest/installing-solidity.html).

## Writing a Smart Contract

Here's a basic example to illustrate the structure and syntax of a Solidity smart contract. This code is for educational purposes and not intended for production use.

```solidity
pragma solidity ^0.8.20;

contract HelloWorld {
    mapping(address => uint) userData;

    function set(uint x) public {
        userData[msg.sender] = x;
    }

    function get() public view returns (uint) {
        return userData[msg.sender];
    }

    function getUserData(address user) public view returns (uint) {
        return userData[user];
    }
}
```

For more information concerning the syntax and semantics of the Solidity language, please refer to the [Solidity documentation](https://docs.soliditylang.org/).

## Compiling, Deploying, and Executing

The command-line compiler solc can be used to compile Solidity code, generating outputs such as binaries, assembly, or an abstract syntax tree. Here are some compilation examples for a file named `HelloWorld.sol`:


```bash
solc --bin HelloWorld.sol   # Output as binary, i.e., bytecode.
solc -o output --bin --ast --asm HelloWorld.sol   # Output in various formats
solc --optimize --bin HelloWorld.sol   # Activate optimizer, for better performance
```

For more detailed guidance, refer to resources like:

- [Using the Solidity command-line compiler](https://docs.soliditylang.org/en/latest/using-the-compiler.html)
- [Compiling contracts using Remix](https://remix-ide.readthedocs.io/en/stable/compile.html)
- [Running transactions with Remix](https://remix-ide.readthedocs.io/en/stable/run.html)
- [Remix Learneth Tutorials](https://remix-ide.readthedocs.io/en/latest/remix_tutorials_learneth.html)

## Debugging Smart Contracts

Debugging Solidity code can be challenging due to limited debugging tools. Here are some resources to assist:

- [Debugging a transaction with Remix](https://remix-ide.readthedocs.io/en/latest/debugger.html)
- [Tutorial on debugging transactions with Remix](https://remix-ide.readthedocs.io/en/latest/tutorial_debug.html)

## Smart Contract Best Practices

Adhering to best practices in Solidity programming is crucial for security and code quality. For reference: [Smart Contract Security Best Practices](https://github.com/ConsenSys/smart-contract-best-practices)
