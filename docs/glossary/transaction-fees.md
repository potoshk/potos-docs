# Transaction fees

## Overview

The fee for a blockchain transaction is calculated using the following formula:

```text
Transaction Fee = (Gas Used) * (Gas Price)
```

To illustrate, suppose you're filling up gas at a gas station. The gas price is determined by the refinery every day, and today's price is \$2. If you fill 15L up, then you would pay \$30 = 15L x \$2/1L for it, and the \$30 will be paid out of your bank account.

Similarly, suppose a transaction spent 21000 gas and the gas price of the transaction was 25 Gwei. Then the gas fee is 525000 Gwei. This amount would be deducted from the sender (`from` account) balance.

## Gas price

The gas price is the amount of price you are willing to pay for each unit of gas. The unit of gas price is literally refered to [Ethereum Unit](https://www.ethereum-ecosystem.com/unit-converter).

For example, three main units are:

- Wei: The smallest unit of Ether
- Gwei: Equal to 1,000,000,000 Wei
- Ether: The main unit, equal to 1,000,000,000 Gwei or 1,000,000,000,000,000,000 Wei

## Gas used

Every state-changing action on the blockchain requires gas. For instance, when executing smart contracts or using tokens like ERC-20, the sender must pay for the computation and storage. The amount paid is determined by the gas required. The gas has no unit, and we just say like "21000 gas".

The actual gas used is determined post-execution and can be retrieved from the transaction receipt.

## Gas limit

The gas limit is the maximum gas units one is willing to spend on a transaction. If you set a limit of 50,000 for a transfer that uses 21,000 gas, you get 29,000 gas back. However, if the limit is too low (e.g., 20,000 for a transfer), the transaction will not complete, and the gas is still consumed, resulting in no state change but a deduction in the sender's balance.

## POTOS specific configuration on gas

The gas price is controlled by POTOS system configuration, and it can be adjusted by the system administrator.  For more details on POTOS system configuration, refer to the [POTOS System Config](../nodes/configure_node.md#system-parameters).
