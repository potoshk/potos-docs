# Governance System Usage Guide

Key to Permission Governance: Control of Keys

- If the keys are in the hands of operators who can use the `console` to operate the blockchain, refer to Section 1 below.
- If the keys are in the hands of individual users or there are no conditions to use the console to operate the blockchain, operations can be performed through `Web3 JSON RPC`, refer to Section 2 below.

## 1. Operator Chain Governance Operations

### 1.1 Console Usage

The console provides exclusive commands for permission governance and commands to switch console accounts. Users can operate permission governance through the console.

Console operation commands include the following types:

- Query status commands, which have no permission control and are accessible to all accounts.
- Governance committee exclusive commands, which can only be used by accounts holding governance committee members.
- Contract administrator exclusive commands, which can only be accessed by administrator accounts with management permissions for a specific contract.

### 1.2 Query Status Commands

#### 1.2.1 getCommitteeInfo

During initialization, a governance committee is deployed. The address information of this committee is automatically generated or specified during the build_chain.sh process. Initially, there is only one committee member with a weight of 1.

```shell
[group0]: /apps> getCommitteeInfo 
---------------------------------------------------------------------------------------------
Committee address   : 0xcbc22a496c810dde3fa53c72f575ed024789b2cc
ProposalMgr address : 0xa0974646d4462913a36c986ea260567cf471db1f
---------------------------------------------------------------------------------------------
ParticipatesRate: 0% , WinRate: 0%
---------------------------------------------------------------------------------------------
Governor Address                                        | Weight
index0 : 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6     | 1
```

#### 1.2.2 getProposalInfo

Batch retrieval of proposal information within a specific range; if only one ID is entered, it returns the proposal information for that single ID.

`proposalType` and `status` show the type and status of the proposal.

`proposalType` includes the following:

- setWeight: Generated when a governance committee initiates an updateGovernorProposal.
- setRate: Generated for setRateProposal.
- setDeployAuthType: Generated for setDeployAuthTypeProposal.
- modifyDeployAuth: Generated for openDeployAuthProposal and closeDeployAuthProposal.
- resetAdmin: Generated for resetAdminProposal.
- setConfig: Generated for setSysConfigProposal.
- setNodeWeight: Generated for addObserverProposal, addSealerProposal, setConsensusNodeWeightProposal.
- removeNode: Generated for removeNodeProposal.
- unknown: Indicates a possible bug.

`status` includes the following:

- notEnoughVotes: The proposal is normal and has not yet collected enough votes.
- finish: The proposal has been executed.
- failed: The proposal has failed.
- revoke: The proposal has been withdrawn.
- outdated: The proposal has exceeded the voting period.
- unknown: Indicates a possible bug.

```shell
[group0]: /apps> getProposalInfo 1
Show proposal, ID is: 1
---------------------------------------------------------------------------------------------
Proposer: 0x4a37eba43c66df4b8394abdf8b239e3381ea4221
Proposal Type   : setWeight
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x4a37eba43c66df4b8394abdf8b239e3381ea4221
---------------------------------------------------------------------------------------------
Against Voters:

# Batch retrieval
[group0]: /apps> getProposalInfo 1 2
Proposal ID: 1
---------------------------------------------------------------------------------------------
Proposer: 0x4a37eba43c66df4b8394abdf8b239e3381ea4221
Proposal Type   : setWeight
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x4a37eba43c66df4b8394abdf8b239e3381ea4221
---------------------------------------------------------------------------------------------
Against Voters:
```

#### 1.2.3 getLatestProposal

To avoid proposal timeouts and forgetting proposal IDs when exiting the console, the `getLatestProposal` command can retrieve the latest proposal information of the current committee.

```shell
[group0]: /apps> getLatestProposal 
Latest proposal ID: 9
---------------------------------------------------------------------------------------------
Proposer: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Proposal Type   : resetAdmin
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
---------------------------------------------------------------------------------------------
Against Voters:
```

#### 1.2.4 getDeployAuth

Permission policies include:

- No permission: Everyone can deploy.
- Blacklist: Users on the blacklist cannot deploy.
- Whitelist: Only users on the whitelist can deploy.

```shell
[group0]: /apps> getDeployAuth
There is no deploy strategy, everyone can deploy contracts.
```

Exclusive commands for the governance committee, which can only be invoked by accounts within the Governors of the governance committee.

If there is only one governance committee member, and the proposal is initiated by that member, the proposal will definitely succeed.

#### 1.2.5 checkDeployAuth

Check if an account has deployment permission.

```shell
# Current deployment permission is in whitelist mode
[group0]: /apps> getDeployAuth 
Deploy strategy is White List Access.

# If no parameter is selected, check if the current account has deployment permission
[group0]: /apps> checkDeployAuth 
Deploy : PERMISSION DENIED
Account: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6

# Check if another account has permission
[group0]: /apps> checkDeployAuth 0xea9b0d13812f235e4f7eaa5b6131794c9c755e9a
Deploy : ACCESS
Account: 0xea9b0d13812f235e4f7eaa5b6131794c9c755e9a
```

#### 1.2.6 getContractAdmin

Use the command to obtain the administrator of a contract, and only the administrator can perform permission control operations on the contract.

```shell
# The admin account for contract address 0xCcEeF68C9b4811b32c75df284a1396C7C5509561 is 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
[group0]: /apps> getContractAdmin 0xCcEeF68C9b4811b32c75df284a1396C7C5509561
Admin for contract 0xCcEeF68C9b4811b32c75df284a1396C7C5509561 is: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
```

#### 1.2.7 checkMethodAuth

Check if an account has the permission to call a specific contract interface.

```shell
# Set the set(string) interface of contract address 0x600E41F494CbEEd1936D5e0a293AEe0ab1746c7b to whitelist mode
[group0]: /apps> setMethodAuth 0x600E41F494CbEEd1936D5e0a293AEe0ab1746c7b set(string) white_list
{
    "code":0,
    "msg":"Success"
}

# If no parameter is selected, check if the current account has the right to call
[group0]: /apps> checkMethodAuth 0x600E41F494CbEEd1936D5e0a293AEe0ab1746c7b set(string)
Method   : PERMISSION DENIED
Account  : 0xea9b0d13812f235e4f7eaa5b6131794c9c755e9a
Interface: set(string)
Contract : 0x600E41F494CbEEd1936D5e0a293AEe0ab1746c7b

# Check the permission of another account
[group0]: /apps> checkMethodAuth 0x600E41F494CbEEd1936D5e0a293AEe0ab1746c7b set(string) 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Method   : ACCESS
Account  : 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Interface: set(string)
Contract : 0x600E41F494CbEEd1936D5e0a293AEe0ab1746c7b
```

#### 1.2.8 getMethodAuth

Obtain all access lists for a specific contract interface.

```shell
# Enable account access to the set interface of contract 0x6849F21D1E455e9f0712b1e99Fa4FCD23758E8F1
[group0]: /apps> openMethodAuth 0x600E41F494CbEEd1936D5e0a293AEe0ab1746c7b "set(string)" 0x5298906acaa14e9b1a3c1462c6938e044bd41967
{
    "code":0,
    "msg":"Success"
}
# Obtain the permission list for the set interface of contract 0x6849F21D1E455e9f0712b1e99Fa4FCD23758E8F1
[group0]: /apps> getMethodAuth 0x600E41F494CbEEd1936D5e0a293AEe0ab1746c7b "set(string)"
---------------------------------------------------------------------------------------------
Contract address: 0x600E41F494CbEEd1936D5e0a293AEe0ab1746c7b
Contract method : set(string)
Method auth type: WHITE_LIST
---------------------------------------------------------------------------------------------
Access address:
5298906acaa14e9b1a3c1462c6938e044bd41967
---------------------------------------------------------------------------------------------
Block address :
```

#### 1.2.9 getContractStatus

Obtain the status of a specific contract, currently only two statuses are available (frozen, normal access).

```shell
[group0]: /apps> getContractStatus 0x31eD5233b81c79D5adDDeeF991f531A9BBc2aD01
Available

[group0]: /apps> freezeContract 0x31eD5233b81c79D5adDDeeF991f531A9BBc2aD01
{
    "code":0,
    "msg":"Success"
}

[group0]: /apps> getContractStatus 0x31eD5233b81c79D5adDDeeF991f531A9BBc2aD01
Unavailable
```

### 1.3 Governance Committee Exclusive Commands

These commands can only be used by accounts holding governance committee members.

#### 1.3.1 updateGovernorProposal

If adding a new governance committee member, simply add the address and weight.

If removing a governance committee member, set the weight of a governance committee member to 0.

```shell
[group0]: /apps> updateGovernorProposal 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6 2
Update governor proposal created, ID is: 1
---------------------------------------------------------------------------------------------
Proposer: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Proposal Type   : setWeight
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
---------------------------------------------------------------------------------------------
Against Voters:
```

#### 1.3.2 setRateProposal

Set proposal thresholds, which are divided into participation thresholds and weight thresholds.

Thresholds are used as percentages here.

```shell
[group0]: /apps> setRateProposal 51 51
Set rate proposal created, ID is: 2
---------------------------------------------------------------------------------------------
Proposer: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Proposal Type   : setRate
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
---------------------------------------------------------------------------------------------
Against Voters:
```

#### 1.3.3 setDeployAuthTypeProposal

Set the deployment ACL policy, supporting only white_list and black_list strategies.

```shell
[group0]: /apps> setDeployAuthTypeProposal white_list
Set deploy auth type proposal created, ID is: 4
---------------------------------------------------------------------------------------------
Proposer: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Proposal Type   : setDeployAuthType
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
---------------------------------------------------------------------------------------------
Against Voters:

[group0]: /apps> getDeployAuth 
Deploy strategy is White List Access.
```

#### 1.3.4 openDeployAuthProposal

Proposal to open deployment permissions for an administrator.

```shell
# Under whitelist mode, only users on the whitelist can use it
[group0]: /apps> deploy HelloWorld 
deploy contract for HelloWorld failed!
return message: Permission denied
return code:18
Return values:null

# Governance committee opens its own (or another account's) deployment permissions
[group0]: /apps> openDeployAuthProposal 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Open deploy auth proposal created, ID is: 5
---------------------------------------------------------------------------------------------
Proposer: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Proposal Type   : modifyDeployAuth
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
---------------------------------------------------------------------------------------------
Against Voters:

# Redeployment is successful
[group0]: /apps> deploy HelloWorld 
transaction hash: 0x732e29baa263fc81e17a93a4102a0aa1cc2ec33ffbc1408c6b206d124b7f7ae0
contract address: 0xCcEeF68C9b4811b32c75df284a1396C7C5509561
currentAccount: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
```

#### 1.3.5 closeDeployAuthProposal

Proposal to close deployment permissions for an administrator.

```shell
# The account can deploy contracts
[group0]: /apps> deploy HelloWorld 
transaction hash: 0x732e29baa263fc81e17a93a4102a0aa1cc2ec33ffbc1408c6b206d124b7f7ae0
contract address: 0xCcEeF68C9b4811b32c75df284a1396C7C5509561
currentAccount: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6

# Close the account's deployment permissions
[group0]: /apps> closeDeployAuthProposal 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6 
Close deploy auth proposal created, ID is: 6
---------------------------------------------------------------------------------------------
Proposer: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Proposal Type   : modifyDeployAuth
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
---------------------------------------------------------------------------------------------
Against Voters:

# The account can no longer deploy contracts
[group0]: /apps> deploy HelloWorld 
deploy contract for HelloWorld failed!
return message: Permission denied
return code:18
Return values:null
```

#### 1.3.6 resetAdminProposal

Proposal to reset the administrator of a contract.

```shell
# The admin account for contract address 0xCcEeF68C9b4811b32c75df284a1396C7C5509561 is 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
[group0]: /apps> getContractAdmin 0xCcEeF68C9b4811b32c75df284a1396C7C5509561
Admin for contract 0xCcEeF68C9b4811b32c75df284a1396C7C5509561 is: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6

# Reset the admin account for 0xCcEeF68C9b4811b32c75df284a1396C7C5509561 to 0xea9b0d13812f235e4f7eaa5b6131794c9c755e9a
[group0]: /apps> resetAdminProposal 0xea9b0d13812f235e4f7eaa5b6131794c9c755e9a  0xCcEeF68C9b4811b32c75df284a1396C7C5509561
Reset contract admin proposal created, ID is: 8
---------------------------------------------------------------------------------------------
Proposer: 0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
Proposal Type   : resetAdmin
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x7fb008862ff69353a02ddabbc6cb7dc31683d0f6
---------------------------------------------------------------------------------------------
Against Voters:

[group0]: /apps> getContractAdmin 0xCcEeF68C9b4811b32c75df284a1396C7C5509561
Admin for contract 0xCcEeF68C9b4811b32c75df284a1396C7C5509561 is: 0xea9b0d13812f235e4f7eaa5b6131794c9c755e9a
```

#### 1.3.7 addSealerProposal

Initiate a proposal to add a new consensus node Sealer.

```shell
# Add a new consensus node Sealer with a weight of 1
[group0]: /apps> addSealerProposal 6471685bb764ddd625c8855809ae23ae803fcf2890977def7c3d4353e22633cdea92471ba0859fc0538679c31b89577e1dd13b292d6538acff42ac4c599d5ce8 1
Add consensus sealer proposal created, ID is: 3
---------------------------------------------------------------------------------------------
Proposer: 0x4a37eba43c66df4b8394abdf8b239e3381ea4221
Proposal Type   : setNodeWeight
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x4a37eba43c66df4b8394abdf8b239e3381ea4221
---------------------------------------------------------------------------------------------
Against Voters:

# Confirm successful addition
[group0]: /apps> getSealerList
[
    Sealer{
        nodeID='63a2e45b2d84f83b32342f0741ffc51069c74fb7c82b8eb0247b12230d50169b86545ecf84420adeec86c57dbc48db1342f4afebc6a127b481eeaaa23722fff0',
        weight=1
    },
    Sealer{
        nodeID='6471685bb764ddd625c8855809ae23ae803fcf2890977def7c3d4353e22633cdea92471ba0859fc0538679c31b89577e1dd13b292d6538acff42ac4c599d5ce8',
        weight=1
    }
]
```

#### 1.3.8 addObserverProposal

Initiate a proposal to add a new observer node Observer.

```shell
[group0]: /apps> addObserverProposal 6471685bb764ddd625c8855809ae23ae803fcf2890977def7c3d4353e22633cdea92471ba0859fc0538679c31b89577e1dd13b292d6538acff42ac4c599d5ce8
Add observer proposal created, ID is: 2
---------------------------------------------------------------------------------------------
Proposer: 0x4a37eba43c66df4b8394abdf8b239e3381ea4221
Proposal Type   : setNodeWeight
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x4a37eba43c66df4b8394abdf8b239e3381ea4221
---------------------------------------------------------------------------------------------
Against Voters:

# Confirm the observer node has joined
[group0]: /apps> getObserverList
[
 6471685bb764ddd625c8855809ae23ae803fcf2890977def7c3d4353e22633cdea92471ba0859fc0538679c31b89577e1dd13b292d6538acff42ac4c599d5ce8
]
```

#### 1.3.9 setConsensusNodeWeightProposal

Initiate a proposal to modify the weight of a consensus node.

```shell
[group0]: /apps> setConsensusNodeWeightProposal 6471685bb764ddd625c8855809ae23ae803fcf2890977def7c3d4353e22633cdea92471ba0859fc0538679c31b89577e1dd13b292d6538acff42ac4c599d5ce8 2
Set consensus weight proposal created, ID is: 6
---------------------------------------------------------------------------------------------
Proposer: 0x4a37eba43c66df4b8394abdf8b239e3381ea4221
Proposal Type   : setNodeWeight
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x4a37eba43c66df4b8394abdf8b239e3381ea4221
---------------------------------------------------------------------------------------------
Against Voters:

# Confirm the weight has been updated to 2
[group0]: /apps> getSealerList
[
    Sealer{
        nodeID='63a2e45b2d84f83b32342f0741ffc51069c74fb7c82b8eb0247b12230d50169b86545ecf84420adeec86c57dbc48db1342f4afebc6a127b481eeaaa23722fff0',
        weight=1
    },
    Sealer{
        nodeID='6471685bb764ddd625c8855809ae23ae803fcf2890977def7c3d4353e22633cdea92471ba0859fc0538679c31b89577e1dd13b292d6538acff42ac4c599d5ce8',
        weight=2
    }
]
```

#### 1.3.10 removeNodeProposal

Initiate a proposal to remove a node.

```shell
[group0]: /apps> removeNodeProposal 6471685bb764ddd625c8855809ae23ae803fcf2890977def7c3d4353e22633cdea92471ba0859fc0538679c31b89577e1dd13b292d6538acff42ac4c599d5ce8
Remove node proposal created, ID is: 7
---------------------------------------------------------------------------------------------
Proposer: 0x4a37eba43c66df4b8394abdf8b239e3381ea4221
Proposal Type   : removeNode
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x4a37eba43c66df4b8394abdf8b239e3381ea4221
---------------------------------------------------------------------------------------------
Against Voters:

[group0]: /apps> getSealerList
[
    Sealer{
        nodeID='63a2e45b2d84f83b32342f0741ffc51069c74fb7c82b8eb0247b12230d50169b86545ecf84420adeec86c57dbc48db1342f4afebc6a127b481eeaaa23722fff0',
        weight=1
    }
]
```

#### 1.3.11 setSysConfigProposal

Initiate a proposal to change system configurations.

```shell
# Change tx_count_limit to 2000
[group0]: /apps> setSysConfigProposal tx_count_limit 2000
Set system config proposal created, ID is: 9
---------------------------------------------------------------------------------------------
Proposer: 0x4a37eba43c66df4b8394abdf8b239e3381ea4221
Proposal Type   : setConfig
Proposal Status : finished
---------------------------------------------------------------------------------------------
Agree Voters:
0x4a37eba43c66df4b8394abdf8b239e3381ea4221
---------------------------------------------------------------------------------------------
Against Voters:

# Check if it has been successfully changed
[group0]: /apps> getSystemConfigByKey tx_count_limit
2000
```

#### 1.3.12 upgradeVoteProposal

Initiate a proposal to upgrade the voting calculation logic. The process of upgrading the proposal voting calculation logic includes the following steps:

1. Write a contract based on the interface;

2. Deploy the written contract on the chain and obtain the contract address;

3. Initiate a proposal to upgrade the voting calculation logic, input the contract address as a parameter, and vote on it within the governance committee;

4. If the vote passes (the voting calculation logic is still the original logic at this point), then upgrade the voting calculation logic; otherwise, do not upgrade.

   The voting calculation logic contract must be implemented according to a certain interface to be usable. The contract implementation can refer to the following interface contract `VoteComputerTemplate.sol` for implementation:

   ```solidity
   // SPDX-License-Identifier: Apache-2.0
   pragma solidity >=0.6.10 <0.8.20;
   
   import "./Committee.sol";
   import "./BasicAuth.sol";
   
   abstract contract VoteComputerTemplate is BasicAuth {
       // Governors and threshold
       Committee public _committee;
   
       constructor(address committeeMgrAddress, address committeeAddress) {
           setOwner(committeeMgrAddress);
           _committee = Committee(committeeAddress);
           // First, test if the committee exists; second, test if the committee is healthy
           require(
               _committee.getWeights() >= 1,
               "committee is error, please check address!"
           );
       }
       // This is the only entry for voting weight calculation logic, which must implement the interface and stipulate:
       // Not enough votes, return 1; vote passes, return 2; vote does not pass, return 3;
       function determineVoteResult(
           address[] memory agreeVoters,
           address[] memory againstVoters
       ) public view virtual returns (uint8);
       
       // This is the verification interface for the calculation logic, used by other governors to verify the validity of the contract
       function voteResultCalc(
           uint32 agreeVotes,
           uint32 doneVotes,
           uint32 allVotes,
           uint8 participatesRate,
           uint8 winRate
       ) public pure virtual returns (uint8);
   }
   ```

   There is an existing contract implemented based on the above `VoteComputerTemplate.sol` interface:

   ```solidity
   // SPDX-License-Identifier: Apache-2.0
   pragma solidity >=0.6.10 <0.8.20;
   
   import "./Committee.sol";
   import "./VoteComputerTemplate.sol";
   
   contract VoteComputer is VoteComputerTemplate {
       constructor(address committeeMgrAddress, address committeeAddress)
           public
           VoteComputerTemplate(committeeMgrAddress, committeeAddress)
       {}
       // Implementation of voting weight calculation logic
       function determineVoteResult(
           address[] memory agreeVoters,
           address[] memory againstVoters
       ) public view override returns (uint8) {
           uint32 agreeVotes = _committee.getWeights(agreeVoters);
           uint32 doneVotes = agreeVotes + _committee.getWeights(againstVoters);
           uint32 allVotes = _committee.getWeights();
           return
               voteResultCalc(
                   agreeVotes,
                   doneVotes,
                   allVotes,
                   _committee._participatesRate(),
                   _committee._winRate()
               );
       }
       // Implementation of the verification interface for the calculation logic
       function voteResultCalc(
           uint32 agreeVotes,
           uint32 doneVotes,
           uint32 allVotes,
           uint8 participatesRate,
           uint8 winRate
       ) public pure override returns (uint8) {
           //1. Checks enough voters: totalVotes/totalVotesPower >= p_rate/100
           if (doneVotes * 100 < allVotes * participatesRate) {
               //not enough voters, need more votes
               return 1;
           }
           //2. Checks whether for votes wins: agreeVotes/totalVotes >= win_rate/100
           if (agreeVotes * 100 >= winRate * doneVotes) {
               return 2;
           } else {
               return 3;
           }
       }
   }
   ```

   After the contract is written, it can be deployed on the chain and updated to the governance committee:

   ```shell
   # First, confirm the Committee contract address is 0xa0974646d4462913a36c986ea260567cf471db1f using the getCommitteeInfo command
   [group0]: /apps> getCommitteeInfo
   ---------------------------------------------------------------------------------------------
   Committee address   : 0xa0974646d4462913a36c986ea260567cf471db1f
   ProposalMgr address : 0x2568bd207f50455f1b933220d0aef11be8d096b2
   ---------------------------------------------------------------------------------------------
   ParticipatesRate: 0% , WinRate: 0%
   ---------------------------------------------------------------------------------------------
   Governor Address                                        | Weight
   index0 : 0x4a37eba43c66df4b8394abdf8b239e3381ea4221     | 2
   
   # Deploy the VoteComputer contract, the first parameter 0x10001 is a fixed address, and the second parameter is the current governance committee Committee's address
   [group0]: /apps> deploy VoteComputer 0x10001 0xa0974646d4462913a36c986ea260567cf471db1f
   transaction hash: 0x429a7ceccefb3a4a1649599f18b60cac1af040cd86bb8283b9aab68f0ab35ae4
   contract address: 0x6EA6907F036Ff456d2F0f0A858Afa9807Ff4b788
   currentAccount: 0x4a37eba43c66df4b8394abdf8b239e3381ea4221
   
   # After deployment, you can update through upgradeVoteProposal
   [group0]: /apps> upgradeVoteProposal 0x6EA6907F036Ff456d2F0f0A858Afa9807Ff4b788
   Upgrade vote computer proposal created, ID is: 10
   ---------------------------------------------------------------------------------------------
   Proposer: 0x4a37eba43c66df4b8394abdf8b239e3381ea4221
   Proposal Type   : upgradeVoteCalc
   Proposal Status : finished
   ---------------------------------------------------------------------------------------------
   Agree Voters:
   0x4a37eba43c66df4b8394abdf8b239e3381ea4221
   ---------------------------------------------------------------------------------------------
   Against Voters:
   
   [group0]: /apps>
   ```

   ## 2. Web3 Method for Chain Governance Operations

   Open Remix and use the git feature to pull the `bcos-auth` repository.

   ![](../_static/develop/remix-import-bcos-auth.png)

   Select CommitteeManager.sol, compile, and point to the contract address `0x0000000000000000000000000000000000010001`

   ![](../_static/develop/remix-at-bcos-auth.png)

   View the committee contract address and the proposal manager's contract address

   ![](../_static/develop/remix-committee-address.png)

   Select PrposalManager.sol, compile, and point to the contract address obtained earlier

   ![](../_static/develop/remix-proposal-contract.png)

   Call the ProposalManager interface to view the specific information of the proposal

   ![](../_static/develop/remix-get-proposal-info.png)

   Call the CommitteeManager interface to vote on the proposal

   ![](../_static/develop/remix-vote-proposal.png)
