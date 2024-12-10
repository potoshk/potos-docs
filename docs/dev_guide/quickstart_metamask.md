# Guide to MetaMask & Connecting to the Testnet

## Goal
This guide will walk you through the process of setting up MetaMask and connecting to the Testnet. 
This is a necessary step for developers who want to interact with Dapps or smart contracts on our network.

## Overview
- Install MetaMask
- Create a MetaMask account
- Connect to the Testnet
- Get Testnet tokens

### Step 1: Install MetaMask(Chrome Extension)
1. Google "MetaMask Chrome Extension" or go to the [MetaMask download page](https://metamask.io/download/) and click "Install MetaMask for Chrome". ![[image]](./imgs/download_metamask.png)
2. Click **"Add to Chrome"**. ![[image]](./imgs/add_to_chrome.png)
3. A popup will appear. Click **"Add Extension"** to confirm.
4. Once installed, the MetaMask fox icon will appear in your browser toolbar. ![[image]](./imgs/metamask_toolbar.png)

### Step 2: Create a MetaMask Account
1. Click on the MetaMask fox icon in the toolbar to open the extension.
2. Check Terms of Use agreement & Select **"Create a new wallet"**. ![[image]](./imgs/create_wallet.png)
3. Agree or disagree to the MetaMask data collection option (this is optional and won't affect functionality).
4. Set a strong password for your wallet and click **"Create a new wallet"**. ![[image]](./imgs/set_password.png)
5. MetaMask will give an option to set up secret recovery phrase:
   - **"Remind me later"** will skip this step.
   - **"Secure my wallet"** will take you to the next step.
6. MetaMask will display a group 12 words as **Secret Recovery Phrase**:
   - **Write down the phrase in order** and keep it safe.
   - Never share this phrase with anyone. ![[image]](./imgs/secret_recovery_phrase.png)
7. Confirm your Secret Recovery Phrase by fill the missing words in the correct order. ![[image]](./imgs/confirm_phrase.png) 
8. Click **"Done"** to complete account creation. ![[image]](./imgs/done.png)

### Step 3: Connect to the Testnet
1. Open MetaMask on Chrome extension or mobile app and unlock it using your password.
2. Click on the **Network Selector** (top-left dropdown that shows "Ethereum Mainnet" by default). ![[image]](./imgs/select_network.png)
3. Select **"+ Add a custom network"**. ![[image]](./imgs/add_new_network.png)
4. Fill in the details for our Testnet & Click **"Save"**: 
   
   | Entry Name         	| Value                       	| Note                         	|
   |--------------------	|-----------------------------	|------------------------------	|
   | Network name       	| Testnet                     	| Name of network              	|
   | Default RPC URL    	| https://rpc.eightart.hk/    	| RPC URL of network           	|
   | Chain ID           	| 20200                       	| blockchain's chain ID        	|
   | Currency symbol    	| HKC                         	| token symbol                 	|
   | Block explorer URL 	| https://scan.eight-art.com/ 	| URL of blockchain's explorer 	|
   
   ![[image]](./imgs/network_detail.png)


### Step 4: Get Testnet Tokens
1. Log in and switch to Testnet in MetaMask app ![[image]](./imgs/metamask_app_home.png) ![[image]](./imgs/metamask_app_switch_testnet.png)
2. Go to the [Faucet](https://faucet.eightart.hk).
3. Click **"Connect Wallet"** and scan with MetaMask app. ![[image]](./imgs/faucet_home.png)
4. Information will be automatically filled in. Click **"Request tokens"**. ![[image]](./imgs/request_tokens.png)
5. Wait few moments and check wallet balance.

You are now ready to interact with Dapps and smart contracts on our network!