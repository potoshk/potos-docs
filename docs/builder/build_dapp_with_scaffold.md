# Build Your First dApp using Scaffold-ETH 2

Scaffold-ETH 2 provides a comprehensive development toolkit for building decentralized applications on EVM-compatible blockchains. This guide demonstrates how to leverage Scaffold-ETH 2 to create and deploy dApps on the POTOS network.

## What is Scaffold-ETH 2?

Scaffold-ETH 2 is an open-source framework that combines multiple modern web3 development tools into a unified workflow. It integrates:

- **Next.js** for building React-based frontends
- **Hardhat** or **Foundry** for smart contract development
- **RainbowKit** and **Wagmi** for wallet connectivity
- **TypeScript** for type-safe development

This combination enables developers to quickly scaffold, develop, test, and deploy full-stack decentralized applications.

## Prerequisites

Before starting, ensure you have the following:

- [Node.js](https://nodejs.org/en/download/) version 18.17 or higher
- [Yarn](https://yarnpkg.com/getting-started/install) package manager (v1 or v2+)
- Basic understanding of JavaScript, React, and Solidity
- [MetaMask](https://metamask.io/download/) wallet extension installed
- Testnet POT tokens from the [POTOS Faucet](https://faucet-testnet.potos.hk)
- POTOS network RPC endpoint (see [RPC Endpoints](../references/rpc_endpoints.md))

## Quick Start

### Step 1: Create a New Project

Initialize a new Scaffold-ETH 2 project using the official CLI:

```bash
npx create-eth@latest
```

The CLI will prompt you with several configuration options:

1. **Project Name**: Choose a descriptive name for your project (e.g., `potos-dapp-example`)
2. **Solidity Framework**: Select either Hardhat or Foundry (this guide uses Hardhat)
3. **Install Dependencies**: Confirm to install required packages

After the installation completes, navigate into your project directory:

```bash
cd your-project-name
```

### Step 2: Configure Hardhat for POTOS

Scaffold-ETH 2 uses Hardhat for smart contract compilation and deployment. To configure Hardhat for the POTOS network, you'll need to set up network parameters and environment variables.

For detailed instructions on configuring Hardhat with POTOS, including network settings, environment variables, and deployment scripts, refer to the [Deploy Smart Contract with Hardhat](./deploy_with_hardhat.md) guide.

The key configuration steps include:

1. Setting up the `.env` file with your deployer private key
2. Adding POTOS Testnet network configuration to `hardhat.config.ts`
3. Configuring the default network for deployments

Once Hardhat is configured, you can compile and deploy contracts using:

```bash
yarn compile
yarn deploy
```

### Step 3: Configure the Frontend

After deploying your smart contracts, configure the frontend to connect to the POTOS network. The frontend configuration is located in `packages/nextjs/scaffold.config.ts`.

Since POTOS Testnet is not included in the default wagmi chain list, you'll need to define a custom chain configuration. Update the `scaffold.config.ts` file:

```typescript
import { defineChain } from 'viem';

const potosTestnet = defineChain({
  id: 60600,
  name: 'POTOS Testnet',
  nativeCurrency: {
    decimals: 18,
    name: 'POT',
    symbol: 'POT',
  },
  rpcUrls: {
    default: {
      http: ['https://rpc-testnet.potos.hk/'],
    },
  },
  blockExplorers: {
    default: {
      name: 'POTOS Explorer',
      url: 'https://scan-testnet.potos.hk',
    },
  },
});

// Update targetNetworks in scaffoldConfig
targetNetworks: [potosTestnet],
```

This configuration tells the frontend to:

- Connect to POTOS Testnet (Chain ID: 60600)
- Use the correct RPC endpoint
- Display transactions in the POTOS block explorer

### Step 4: Run Your dApp

With both backend and frontend configured, start the development server:

```bash
yarn start
```

The dApp will be available at `http://localhost:3000`. You can:

- Connect your MetaMask wallet (make sure it's connected to POTOS Testnet)
- Interact with deployed smart contracts
- Use the built-in contract debugger
- Test contract functions through the UI

## Development Workflow

When building with Scaffold-ETH 2 on POTOS, follow this typical workflow:

1. **Write Smart Contracts**: Add your Solidity contracts to `packages/hardhat/contracts/`
2. **Create Deployment Scripts**: Write deployment logic in `packages/hardhat/deploy/`
3. **Deploy to POTOS**: Use Hardhat to deploy contracts to POTOS Testnet
4. **Verify Contracts**: Verify deployed contracts on the block explorer (optional)
5. **Build Frontend**: Customize React components in `packages/nextjs/pages/`
6. **Test Locally**: Run the development server and test interactions
7. **Deploy Frontend**: Deploy your frontend to a hosting service when ready

## Contract Verification

After deploying your contracts, you may want to verify them on the POTOS block explorer. This allows users to view and interact with your contract source code.

To verify contracts using Hardhat's verify plugin, add the following configuration to your `hardhat.config.ts`:

```typescript
etherscan: {
  apiKey: {
    potosTestnet: "unnecessary",
  },
  customChains: [
    {
      network: "potosTestnet",
      chainId: 60600,
      urls: {
        apiURL: "https://scan-testnet.potos.hk/api",
        browserURL: "https://scan-testnet.potos.hk",
      },
    },
  ],
},
```

Then verify your contract:

```bash
yarn hardhat verify --network potosTestnet <CONTRACT_ADDRESS> [CONSTRUCTOR_ARGS]
```

## Next Steps

Now that you have a working dApp scaffold on POTOS, consider:

- Customizing the default contract and frontend components
- Adding your own smart contract logic
- Implementing additional frontend features
- Testing thoroughly on Testnet before mainnet deployment

For more information about Scaffold-ETH 2 features and capabilities, visit the [official documentation](https://docs.scaffoldeth.io/).
