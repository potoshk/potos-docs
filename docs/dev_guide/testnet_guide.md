# Guide to POTOS Testnet

## Apply for Testnet "Balance"

### 1. Create A Wallet
If you do not have a wallet yet, create an account using MetaMask (choose any Ethereum-compatible account type) [here](https://metamask.io/download/). 
For more information on MetaMask, please refer [here](https://docs.metamask.io/). 

### 2. Connect MetaMask to POTOS Testnet

To access the Testnet, follow steps below to configure MetaMask:

1. Open the "Network" setting, click "Add a network":

 <img src="../imgs/connect_1.png" width="600" />

2. Click "Add a network manually":

 <img src="../imgs/connect_2.png" width="600" />

3. Fill in required information for the Testnet and click "Save": 

	| Entry            	| POTOS Testnet               	     |
	|-----------------	|-----------------------------------|
	| Network name    	| hkchain_testnet                 	 |
	| New RPC URL     	| https://rpc-testnet.potos.hk 	    |
	| Chain ID        	| 20200                        	    |
	| Currency symbol 	| FBC                             	 |

 <img src="../imgs/connect_3.png" width="600" />

4. You will see the Testnet network added to the list.


### 3. Receive Testnet Tokens
Supply your wallet address [here](faucet_site.com) to get free Testnet faucet funds. 

Testnet tokens are crypto assets created on a parallel test network to the Mainnet. 
Developers use the Testnet to test smart contracts and transactions without risking real-world implications on the Mainnet. 

These Testnet tokens work like Mainnet coins, covering gas fees during testing. 
However, as they are issued in the parallel environment, Testnet tokens have no real-world value on the Mainnet.

You may find your wallet address here:

 <img src="../imgs/wallet_address.png" width="600" />


## Trading with MetaMask Wallet

Once balance is updated, you can start trading on the Testnet.

 <img src="../imgs/updated_balance.png" width="600" />

* To transfer tokens to another wallet address, make sure that you set a gas limit >= 210,000 WEI.

 <img src="../imgs/transfer.png" width="600" />
 
* Confirm the transaction:

 <img src="../imgs/transfer_confirm.png" width="600" />
 
* Details will be displayed confirmation: 

 <img src="../imgs/transaction_info.png" width="600" />

## Explorer

Check activities taking place on the Testnet [here](https://scan-testnet.potos.hk/weco/).
 

## Advanced Topics

### Interact with Remix

#### Configure Remix to Access Testnet

Configure environment information in `Deployment & Run Transactions` tab [here](https://remix.ethereum.org/). 

Select `Injected Provider - MetaMask` for `Environment`:

<img src="../imgs/remix_config.png" width="600" />

#### Send Transactions via Remix

When initiating a deployment or calling on a contract, Remix will send the contract content to MetaMask.
You will need to confirm transaction on MetaMask. 

Find more information on Remix Online IDE [here](https://remix-ide.readthedocs.io/en/latest/).


### Interact with Hardhat

#### Configure Hardhat to Access Testnet

Set the `IP port` and `ChainID` for the Testnet in `hardhat.config.js` configuration file.

| Entry            	| POTOS Testnet               	  |
|-----------------	|--------------------------------|
| New RPC URL     	| https://rpc-testnet.potos.hk 	 |
| Chain ID        	| 20200                        	 |


Send balance to the address shown in below.

#### Perform Tests Using Hardhat

To run all tests in terminal, execute the following command:

```bash
hardhat test --network localhost
```

For more information on Hardhat, please refer [here](https://hardhat.org/tutorial).
