# Intro to crypto wallet

If you don’t have one yet, check out our [How to create an FISCO BCOS account](../glossary/accounts.md).

## What Is a Crypto Wallet?

A crypto wallet is an interface that allows you to access and manage your funds on a blockchain, including sending and receiving digital assets. In other words, your crypto wallet is the bridge between you and your crypto on the blockchain, allowing you to take part in crypto trading, DeFi platforms, or even NFTs.

However, despite what the name suggests, a crypto wallet does not actually contain your coins or tokens – those stay on the blockchain itself. Rather, the purpose of a cryptocurrency wallet is to secure private keys that allow you to access that crypto.

## What are Crypto Wallets For?

Each account on the blockchain is controlled by a private key. That private key gives the owner access to your blockchain address and everything stored there.

In its raw form, a private key is a long string of 1s and 0s, but this is where the risk comes in: storing a number like that is impractical. How many humans do you know that could record 256 bits in binary?

Besides, all it takes is one person to see this number, and all your crypto is gone. What if you have multiple crypto accounts? You’d need to write and store the private key for each one, amplifying the chances of somebody seeing this precious piece of information and understanding what it means.

This is where crypto wallets come in. They remove the need to record lists of 1s and 0s, eliminating the risks of recording them incorrectly, or simply taking up too much space. They store your private keys so you don’t have to.

## How Do Crypto Wallets Work?

Most crypto wallets can generate and store multiple private keys, meaning you can manage multiple blockchain accounts with a single wallet. That’s because most modern cryptocurrency wallets use an HD structure that allows you to generate and recover your accounts using a single code called a seed phrase (or secret recovery phrase).

This 12 to 24-word mnemonic phrase will let you restore all of your accounts with any compatible cryptocurrency wallet provider, acting as a master key to all of your accounts. Each account in an HD wallet operates separately, controlled by a separate private key. This means when you sign a transaction with one account, it doesn’t affect the other.

Each account you generate has a private key and a corresponding public key. While the private key grants the owner access to the blockchain account, the public key serves as the account’s unique identifier. It’s like a username, allowing the blockchain and its participants to find and send assets to your account.

When you sign transactions using your private key, this verifies that you authorize the terms of the transaction. To execute the transaction, the blockchain nodes verify your account has the funds required to execute your request and that your signature is authentic using your public key. In fact, that’s where your blockchain address comes from, it’s simply a more human-readable translation of your public key.

## Traditional Wallets vs Custodial Wallets vs Non-Custodial Wallets

There are several different kinds of crypto wallets, but the first categories you should understand are custodial and non-custodial wallets.

A custodial wallet is usually issued by a centralized crypto exchange, and it does not give you full control over your funds.

In contrast, non-custodial wallets give you complete control over your private keys and, by extension, your assets. This model ensures you maintain full autonomy over your funds.

Here’s a comparison of traditional wallets, custodial wallets, and non-custodial wallets:

| Criteria                  | Traditional Wallets                                                      | Custodial Crypto Wallets                                                                        | Non-Custodial Crypto Wallets                                                                        |
|---------------------------|--------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Control Over Assets**   | Full control                                                             | Limited control; assets are held by the wallet provider                                         | Full control; users have direct access to their private keys                                        |
| **Physical Form**         | Physical object (e.g., leather)                                          | Digital, often through a web interface or mobile app                                            | Digital, typically installed on personal devices                                                    |
| **Access Method**         | Physical possession (e.g., cash, cards)                                  | Login credentials (username/password)                                                           | Private keys or seed phrases                                                                        |
| **Risk of Loss**          | Risk of physical loss or theft                                           | Risk of loss if the provider fails or is hacked                                                 | Risk of loss if private keys are lost or compromised                                                |
| **Security Measures**     | Physical safeguards (e.g., zippers, clasps)                              | Provider's security infrastructure, including encryption and multi-factor authentication        | User responsibility for securing private keys, often with additional software or hardware solutions |
| **Portability**           | Limited by physical presence                                             | High; accessible from any device with internet connection                                       | High; accessible from any device with the private keys or seed phrases                              |
| **Regulatory Compliance** | Subject to local financial regulations                                   | Subject to both local financial regulations and KYC/AML procedures                              | Less regulated; users bear responsibility for compliance                                            |
| **Transaction Speed**     | Instantaneous                                                            | Can be fast, depending on the provider's infrastructure                                         | Can be slower due to the need to sign transactions manually                                         |
| **Privacy**               | High, unless transactions are traceable (e.g., credit card transactions) | Moderate; transactions are recorded on the blockchain and may be linked to personal information | High; transactions are recorded on the blockchain, but personal information is not required         |
| **Cost**                  | Typically none or minimal for physical wallets                           | May include fees for transactions, storage, or account recovery                                 | May require initial purchase of hardware wallets or software licenses                               |

## Frequently asked questions

### If I own an address, do I own the same address on other blockchains?

You can use the same address on all EVM compatible blockchains (if you have the type of wallet with a recovery phrase). This [list](https://chainlist.org/) will show you which blockchains you can use with the same address. Some blockchains, like Bitcoin, implement a completely separate set of network rules and you will need a different address with a different format. If you have a smart contract wallet you should check its product website for more info on which blockchains are supported.

### Can I use the same address on multiple devices?

Yes, you can use the same address on multiple devices. Wallets are technically only an interface to show you your balance and to make transactions, your account isn't stored inside the wallet, but on the blockchain.

### Can I cancel or return transactions?

No, once a transaction is confirmed, you cannot cancel the transaction.
