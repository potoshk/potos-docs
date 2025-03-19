# Deploy smart contract using Hardhat

## Introduction

This section will guide you through deploying an ERC20 to the Universal BCOS Network using [Hardhat](https://hardhat.org/).

Hardhat is a smart-contract development environment that will help you:

- Develop and compile smart contracts.
- Debug, test, and deploy smart contracts and DApps.

The ERC-20 introduces a standard for Fungible Tokens, in other words, they have a property that makes each Token be exactly
the same (in type and value) as another Token.

By the end of this guide you will be able to:

- Set up a Hardhat project on Universal BCOS.
- Create a simple ERC20 token.
- Compile your smart contract using Hardhat.
- Test, deploy, and interact with your smart contract using Hardhat.
- Explore Hardhat forking feature.

## Prerequisites

To follow this tutorial, the following are the prerequisites:

- Code editor: a source-code editor such [VSCode](https://code.visualstudio.com/download).
- [Metamask](./wallet_usage.md): used to deploy the contracts, sign transactions and interact with the ontracts.
- [Hardhat Requirements](https://hardhat.org/tutorial/setting-up-the-environment): environment required in Hardhat.

## Step 1: Initialize an Hardhat project

```bash
mkdir bcos-hardhat-erc20
cd bcos-hardhat-erc20

# init npm project
npm init -y
# install hardhat
npm install --save-dev hardhat 
```

Paste the code below to install other dependencies.

```bash
npm install dotenv @openzeppelin/contracts
```

In the same directory where you installed Hardhat run:

```bash
npx hardhat init
```

Select `Create a JavaScript project` option and an empty `hardhat.config.js` with your keyboard and hit enter.

```bash
$ npx hardhat init
888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

👷 Welcome to Hardhat v2.22.13 👷‍

✔ What do you want to do? · Create a JavaScript project
✔ Hardhat project root: · /opt/bcos-hardhat-erc20
✔ Do you want to add a .gitignore? (Y/n) · y
✔ Do you want to install this sample project's dependencies with npm (@nomicfoundation/hardhat-toolbox)? (Y/n) · y
```

After initializing a hardhat project, your current directory should include:

**contracts/** – this folder contains smart contract code.

**scripts/** – this folder contains code that deploys your contracts on the blockchain network.

**test/** – this folder contains all unit tests that test your smart contract.

**hardhat.config.js** – this file contains configurations important for the work of Hardhat and the deployment of the soul-bound token.

## Step 2: Setup Hardhat Configs

Now create your .env file in the project folder. This file helps us load environment variables from an .env file into process.env.

Paste this command in your terminal to create a .env file

```bash
touch .env
```

After creating our file, let's configure our .env file to look like this:

```bash
 BCOS_URL= "Your BCOS URL"
 PRIVATE_KEY= "your private key copied from MetaMask wallet"
```

Modify your hardhat.config.js with the following configurations:

```js
require("@nomicfoundation/hardhat-toolbox");
require('dotenv').config()


module.exports = {
  solidity: "0.8.27",
  networks: {
    bcos: {
      url: process.env.BCOS_URL || "",
      gasPrice: 30000000,
      accounts:
        process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : [],
    }
  }
};
```

Now that we have our development environment all set, let's get into writing our ERC20 token smart contract.

## Step 3: Create ERC20 Contract

Create `MyToken.sol` in `contracts/` directory:

```solidity
// SPDX-License-Identifier: Apache 2.0
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol";

contract MyToken is ERC20, ERC20Permit {
    constructor() ERC20("MyToken", "MTK") ERC20Permit("MyToken") {}

    function mint(uint256 value) public {
        _mint(msg.sender, value);
    }
}
```

## Step 4: Compile your contract

To compile the contract run `npx hardhat compile` in your terminal. The compile task is one of the built-in tasks.

```bash
$ npx hardhat compile

Downloading compiler 0.8.27
Compiled 19 Solidity files successfully (evm target: paris).
```

## Step 5: Testing contracts

Create a new directory called `test` inside our project root directory and create a new file in there called `MyToken.js`.

```bash
mkdir -p test
cd test
touch MyToken.js
```

Let's start with the code below. We'll explain it next, but for now paste this into Token.js:

```js
const { expect } = require("chai");

describe("Token contract", function () {
  it("Deployment should assign the total supply of tokens to the owner", async function () {
    const [owner] = await ethers.getSigners();

    const hardhatToken = await ethers.deployContract("MyToken");

    await hardhatToken.mint(10000);
    const ownerBalance = await hardhatToken.balanceOf(owner.address);
    expect(await hardhatToken.totalSupply()).to.equal(ownerBalance);
  });
});

```

In your terminal run `npx hardhat test`. You should see the following output:

```bash
$ npx hardhat test

  Token contract
    ✔ Deployment should assign the total supply of tokens to the owner

  1 passing (382ms)
```

## Step 6: Deploy contracts

Scripts are JavaScript/Typescript files that help you deploy contracts to the blockchain network. In this section, you will create a script for the smart contract.

In the Explorer pane, select the "scripts" folder and click the New File button to create a new file named deploy.js

```js
const { ethers } = require("hardhat");

async function main() {

  const deployerAddr = "[Your Metamask wallet address]";
  const deployer = await ethers.getSigner(deployerAddr);

  console.log(`Deploying contracts with the account: ${deployer.address}`);
  console.log(`Account balance: ${(await deployer.provider.getBalance(deployerAddr)).toString()}`);


  const token = await ethers.deployContract("MyToken");
  await token.waitForDeployment();

  console.log(`Congratulations! You have just successfully deployed your ERC20 tokens.`);
  console.log(`ERC20 contract address is ${token.target}.`);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

In the terminal, run the following command which tells Hardhat to deploy your ERC20 token on the BCOS Network.

```bash
npx hardhat run scripts/deploy.js --network bcos

Deploying contracts with the account: 0xEc5E7dEc9d2d6bfA1f2221aCE01aE3deb6906fb0
Account balance: 100000000000000000000000
Congratulations! You have just successfully deployed your ERC20 tokens.
ERC20 contract address is 0xAd384DC5c3aE98460A8969770373001783963c13.
```
