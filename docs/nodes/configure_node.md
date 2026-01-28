# Configuration

FISCO BCOS configuration encompasses both on-chain and off-chain settings.

- **On-chain configuration** is the unified configuration of the blockchain network. Modification of it requires administrators to send transactions to the chain, and all consensus nodes reach an agreement.
- **Off-chain configuration** refers to individual node configuration options that can be modified by simply updating the operator's configuration files itself without sending transactions.

## On-Chain Configuration Instructions

On-chain configuration includes the genesis block and system settings.

- The genesis block configuration file must be **consistent across all nodes within the group** and **cannot be altered after the chain is initialized**. Even if the genesis block configuration is changed after the chain initialization, the new settings will not take effect. The system must use the fixed genesis configuration after chain initialization.
- System configurations in FISCO BCOS are managed within the built-in precompiled contracts, including system parameter management, consensus node management, permission management, and balance management.

### Genesis Block Configuration

Genesis block configurations are located in the configuration file `config.genesis`.

- **Consistency of the genesis block configuration acrossall nodes in the group is mandatory.**
- **The genesis block configuration file cannot be changed after chain initialization.**
- Any changes to the genesis block configuration after chain initialization will not take effect; the system will still use the genesis configuration from the time of chain initialization.

#### Chain Information Configuration

The `[chain]` configuration section covers node chain information. **Fields under this configuration should not be changed once set**:

- `[chain].sm_crypto`: Whether the node uses SM crypto for the ledger, default is `false`;
- `[chain].group_id`: Group ID, default is `group0`;
- `[chain].chain_id`: Chain ID, default is `chain0`.

```ini
[chain]
    ; use SM crypto or not, should never be changed
    sm_crypto=false
    ; the group id, should never be changed
    group_id=group0
    ; the chain id, should never be changed
    chain_id=chain0
```

#### Consensus Configuration

The `[consensus]` section covers consensus-related configurations, including:

- `[consensus].consensus_type`: The consensus type, default is `pbft`, supports `pbft`, `rpbft`, and can be switched from `pbft` to `rpbft` as needed.
- `[consensus].block_tx_count_limit`: The maximum number of transactions per block, default is 1000. This setting affects the block production rate during peak times and can be adjusted through system configurations. It is recommended to set a higher value during transaction peak times.
- `[consensus].consensus_timeout`: BFT consensus timeout in milliseconds, default is 3000.
- `[consensus].leader_period`: The number of consecutive blocks a leader can pack during the consensus process, default is 1.
- `[consensus].node.idx`: The list of initial consensus nodes' NodeIDs.

A sample `[consensus]` configuration is as follows:

```ini
[consensus]
consensus_type = pbft
block_tx_count_limit = 1000
leader_period = 5
node.0 = 94172c95917fbf47b4b98aba0cc68f83f61a06b0bc373695590f343464b52c9b40d5f4dd98384c037d4cad938b329c6af826f695a7123007b7e06f24c6a48f20:1
node.1 = 74034fb43f75c63bb2259a63f71d9d1c658945409889d3028d257914be1612d1f2e80c4a777cb3e7929a0f0d671eac2fb9a99fa45d39f5451b6357b00c389a84:1
```

#### Data Compatibility Configuration

FISCO BCOS supports data version upgrade dynamically. This configuration item is located under `[version]`:

- `[version].compatibility_version`: Data compatibility version number, default is `v3.10.0`. When a new version upgrade is initiated, all binaries are replaced, and the data version can be dynamically upgraded by sending a transaction to modify the system configuration.

**Special Note:** Since modifying this item will affect the overall behavior of the chain, ensure to back up before proceeding. Confirm that the modification is necessary before making changes.

#### Gas Configuration

To prevent DOS attacks on EVM/WASM, the concept of gas is introduced to measure the consumption of computing and storage resources during the execution of smart contracts. This includes the maximum gas limit for transactions. If the gas consumed by a transaction or block exceeds the limit (gas limit), the transaction is discarded. The `[tx].gas_limit` in the genesis block can configure the maximum gas limit for transactions, defaulting to 3000000000. After the chain is initialized, the gas limit can be dynamically adjusted by sending transactions to modify system configurations.

In actual production, the configuration can be adjust according to machine costs, transaction throughput, and actual business scenarios.

- `[tx].gas_limit`: The gas limit for a single transaction execution, default is set to 3,000,000,000.

A sample `[tx].gas_limit` configuration is as follows:

```ini
[tx]
gas_limit = 3000000000
```

#### Executor Module Configuration

The `[executor]` configuration items involve executor-related genesis block configurations, including:

- `[executor].is_wasm`: Configures the virtual machine type, `true` indicates the use of the WASM virtual machine, `false` indicates the use of the EVM virtual machine. This configuration option cannot be dynamically adjusted and defaults to `false`.
- `[executor].is_auth_check`: The configuration switch for access control, `true` indicates that access control is enabled, `false` indicates that access control is disabled. The access control feature is turned off by default and can be modified later by sending transactions.
- `[executor].is_serial_execute`: The configuration switch for serial and parallel execution modes of transactions, `true` indicates serial execution mode, `false` indicates DMC parallel execution mode. This configuration option cannot be dynamically adjusted and defaults to `true`.
- `[executor].auth_admin_account`: The initial account address of the governance committee, used only in scenarios with access control.

### On-Chain System Configuration

On-Chain System Configuration includes system parameter management, consensus node management, permission management, and balance management. These management features are implemented through built-in precompiled contracts in FISCO BCOS. For more information on the principles of precompiled contracts, please refer to: [Precompiled C++ engine — FISCO BCOS documentation](../../glossary/precompiled.md).

#### System Parameter Management

System parameter management is implemented by the precompiled contract `SystemConfig` with the contract address `0x1000`. The interface is as follows:

```solidity
abstract contract SystemConfigPrecompiled
{
    function setValueByKey(string memory key, string memory value) public returns(int32);   // Write interface
    function getValueByKey(string memory key) public view returns(string memory,int256);    // Read interface
}
```

##### System Parameters

Currently supported system parameters are:

| System Parameter Name          | Parameter Explanation                                                       | Default Value | Special Notes                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------------------------|-----------------------------------------------------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tx_gas_limit                   | The gas limit for each transaction                                          | 3,000,000,000 | Needs to be specially set according to business conditions                                                                                                                                                                                                                                                                                                                                                  |
| tx_gas_price                   | The price per gas in billing mode                                           | 0             | The setting of gas price needs to be specially set based on actual machine costs and business conditions                                                                                                                                                                                                                                                                                                    |
| tx_count_limit                 | The maximum number of transactions in a block                               | 1,000         |                                                                                                                                                                                                                                                                                                                                                                                                             |
| consensus_leader_period        | The number of consecutive blocks a leader can pack in the consensus process | 1             |                                                                                                                                                                                                                                                                                                                                                                                                             |
| auth_check_status              | Whether to perform permission control                                       | 0             | When set to 0, no permission control is performed; other values indicate permission control will be performed. **Special Note:** After enabling permission control, all system configurations and consensus node additions or withdrawals can only be called by address 0x10001, i.e., the governance committee contract address, so they must be modified through governance committee voting.             |
| compatibility_version          | Data version number                                                         | 3.10.0        |                                                                                                                                                                                                                                                                                                                                                                                                             |
| web3_chain_id                  | Web3 RPC's chain id                                                         | 0             | This configuration is the unique identifier for Web3 tool access to the chain and should not be confused with other chains' ids or frequently modified.                                                                                                                                                                                                                                                     |
| balance_transfer               | Whether balance transfers are allowed                                       | 1             | Default support for balance transfers                                                                                                                                                                                                                                                                                                                                                                       |
| feature_rpbft                  | Enable rpbft consensus algorithm                                            | 0             | Cannot be modified after activation.                                                                                                                                                                                                                                                                                                                                                                        |
| feature_rpbft_epoch_block_num  | The interval of epoch block height in rpbft                                 | 1,000         | This value takes effect only after feature_rpbft is enabled. It should not be configured too small, which will significantly affect performance.                                                                                                                                                                                                                                                            |
| feature_rpbft_epoch_sealer_num | The number of nodes elected in each epoch in rpbft                          | 4             | This value takes effect only after feature_rpbft is enabled.                                                                                                                                                                                                                                                                                                                                                |
| feature_dmc2serial             | Support switching from DMC parallel mode to serial mode                     | 0             | Cannot be modified after activation.                                                                                                                                                                                                                                                                                                                                                                        |
| feature_sharding               | Enable sharding block transaction execution mode                            | 0             | Cannot be modified after activation.                                                                                                                                                                                                                                                                                                                                                                        |
| feature_paillier               | Enable Paillier homomorphic encryption feature                              | 0             | Cannot be modified after activation. Users can call contracts at address 0x5003 for homomorphic encryption addition operations.                                                                                                                                                                                                                                                                             |
| feature_balance                | Enable balance feature in Solidity contracts                                | 0             | Cannot be modified after activation.                                                                                                                                                                                                                                                                                                                                                                        |
| feature_balance_precompiled    | Enable balance management feature                                           | 0             | Cannot be modified after activation. This value requires prior activation of `feature_balance`.                                                                                                                                                                                                                                                                                                             |
| feature_balance_policy1        | Enable billing mode                                                         | 0             | Cannot be modified after activation. This value requires prior activation of `feature_balance` and `feature_precompiled`. Once enabled, a fee will be deducted for each transaction. Due to historical reasons, enabling this system configuration alone will not allow balance transfers. Please use it with `balance_transfer`, whose configuration will override the rules of `feature_balance_policy1`. |
| feature_evm_cancun             | Enable Cancun upgrade                                                       | 0             | Cannot be modified after activation. This value, when enabled, will support the latest version of Solidity 0.8.27 and is recommended to be enabled.                                                                                                                                                                                                                                                         |
| feature_evm_timestamp          | Align contract time units to seconds                                        | 0             | Cannot be modified after activation.                                                                                                                                                                                                                                                                                                                                                                        |
| feature_evm_address            | Enable contract address calculation based on Ethereum's rules               | 0             | Cannot be modified after activation.                                                                                                                                                                                                                                                                                                                                                                        |
| feature_rpbft_term_weight      | Enable rpbft election weight feature                                        | 0             | Cannot be modified after activation. This value takes effect only after feature_rpbft is enabled.                                                                                                                                                                                                                                                                                                           |

##### System Configuration Contract Permission Management

After enabling permission control, all write interfaces of the system configuration can only be called by address 0x10001, i.e., the governance committee contract address. Therefore, modifications must be made through governance committee voting. For details, please refer to the "Permission Management" section.

#### Consensus Node Management

Consensus node management is implemented by the precompiled contract `ConsensusPrecompiled` with the contract address `0x1003`. The interface is as follows:

```solidity
abstract contract ConsensusPrecompiled {
    function addSealer(string memory,uint256) public returns (int32);   // Set the specified node type as sealer
    function addObserver(string memory) public returns (int32);         // Set the specified node type as observer
    function remove(string memory) public returns (int32);              // Remove the specified node
    function setWeight(string memory,uint256) public returns (int32);   // Set the weight of the specified node
}
```

This precompiled contract holds a data table with the following structure:

| Field Name   | Field Explanation                                                                                  |
|--------------|----------------------------------------------------------------------------------------------------|
| nodeID       | The unique identity of the node, which is the basis in the consensus process.                      |
| type         | The type of node, currently `consensus_sealer`, `consensus_observer`, `consensus_candidate_sealer` |
| voteWeight   | PBFT consensus voting weight                                                                       |
| termWeight   | rPBFT election weight                                                                              |
| enableNumber | The block height at which it starts to be enabled                                                  |

##### Consensus Node Management Contract Permission Management

After enabling permission control, all write interfaces of consensus node management can only be called by address 0x10001, i.e., the governance committee contract address. Therefore, modifications must be made through governance committee voting. For details, please refer to the "Permission Management" section.

#### Permission Management

The on-chain governance committee contract is a pre-deployed Solidity contract in FISCO BCOS. Its main functions include governance of the governance committee, governance of on-chain configuration management, governance of consensus node management, account management, deployment permission management, and contract permission management.

For detailed design, please see the link: [Committee Design](../../developer/committee.md); for specific operational steps, please see the link: [Committee Usage](../../developer/committee_usage.md).

#### Balance Management

Balance management is implemented by the precompiled contract `BalancePrecompiled` with the contract address `0x1011`. The interface is as follows:

```solidity
abstract contract BalancePrecompiled {
    function getBalance(address account) public view returns (uint256); // Get the balance of a specified account
    function addBalance(address account, uint256 amount) public;        // Increase the balance of a specified account
    function subBalance(address account, uint256 amount) public;        // Decrease the balance of a specified account
    function transfer(address from, address to, uint256 amount) public; // Transfer from a specified from account to a to account
    function registerCaller(address account) public;                    // Register the caller of this contract
    function unregisterCaller(address account) public;                  // Unregister the caller of this contract
    function listCaller() public view returns (address[] memory);       // Get the callers of this contract
}
```

##### Balance Management Contract Permission Management

In the initial stage, only the governance committee can call the write interfaces of this contract. The governance committee can authorize other users to call by calling the `registerCaller` of this contract, and only the governance committee can call the `unregisterCaller` interface to revoke authorization.

## Off-Chain Configuration Instructions

This section including configure file `config.ini`、p2p peer connection config `nodes.json` and certs of connections and consensus in `conf`:

- `config.ini`: The node configuration file, which mainly configures RPC, P2P, SSL certificate paths, ledger data paths, disk encryption, and other information.
- `nodes.json`: The list of peer node URLs that the node actively connects to.
- `conf`: Node consensus and network connection certificates.

### Node Configuration File

The `config.ini` file uses the `ini` format and includes configuration items for **P2P, RPC, cert, chain, security, consensus, storage, txpool, and log**.

- The public IPs of cloud hosts are virtual IPs. If`listen_ip` is set to an external IP, it will fail to bind.You must use `0.0.0.0`.
- RPC/P2P listening ports must be within the range of1024-65535 and should not conflict with other applicationlistening ports on the machine.
- For development and experience convenience, the reference configuration for `listen_ip` is `0.0.0.0`. For security considerations, please modify it to a secure listening address according to the actual business network situation, such as an internal IP or a specific external IP.

#### Configuring P2P

P2P-related configurations include:

- `[p2p].listen_ip`: The IP for P2P listening, default is set to `0.0.0.0`;
- `[p2p].listen_port`: The port for node P2P listening;
- `[p2p].sm_ssl`: Whether to use SM SSL protocol for SSL connections between nodes, `true` indicates the use of SM SSL connections; `false` indicates the use of non-SM SSL connections, default is `false`;
- `[p2p].nodes_path`: The directory where the node connection information file `nodes.json` is located, default is the current folder;
- `[p2p].nodes_file`: The path to the `P2P` connection information file `nodes.json`.

A sample P2P configuration is as follows:

```ini
[p2p]
    listen_ip=0.0.0.0
    listen_port=30300
    ; ssl or sm ssl
    sm_ssl=false
    nodes_path=./
    nodes_file=nodes.json
```

The `p2p` connection configuration file `nodes_file` format:

```shell
{"nodes":[connection list]}
```

Example:

```shell
{"nodes":["127.0.0.1:30300","domain.example.org:30300"]}
```

`P2P` supports configurable network connections and allows for dynamically adding/removing connection nodes during service operation. The process is as follows:

- Modify the connection information in the `[p2p].nodes_file` configuration.
- Send a signal `USR1` to the service process:

```shell
kill -USR1 node_pid
```

The service will reload the `P2P` connection information.

#### Configuring RPC

RPC configuration options are located in the `[rpc]` section and include:

- `[rpc].listen_ip`: The IP for RPC listening, default is set to `0.0.0.0` for convenience in deploying nodes and SDKs across machines.
- `[rpc].listen_port`: The port for RPC listening, default is set to `20200`.
- `[rpc].thread_count`: The number of RPC service threads, default is 4.
- `[rpc].sm_ssl`: Whether to use SM SSL connections between the SDK and nodes, `true` indicates the use of SM SSL connections; `false` indicates the use of non-SM SSL connections, default is `false`.
- `[rpc].disable_ssl`: Whether to disable SSL connections, default is to require SSL connections.

A sample RPC configuration is as follows:

```ini
[rpc]
    listen_ip=0.0.0.0
    listen_port=20200
    thread_count=4
    ; ssl or sm ssl
    sm_ssl=false
    ; ssl connection switch, if disable the ssl connection, default: false
    ;disable_ssl=true
```

#### Configuring Certificate Information

For security considerations, FISCO BCOS nodes use SSL encrypted communication. The `[cert]` configuration sets the SSL connection certificate information:

- `[cert].ca_path`: The certificate path, default is `conf`;
- `[cert].ca_cert`: The CA certificate name, default is `ca.crt`;
- `[cert].node_key`: The node SSL connection private key, default is `ssl.key`;
- `[cert].node_cert`: The node SSL connection certificate, default is `ssl.cert`.

```ini
[cert]
    ; directory the certificates located in
    ca_path=./conf
    ; the ca certificate file
    ca_cert=ca.crt
    ; the node private key file
    node_key=ssl.key
    ; the node certificate file
    node_cert=ssl.crt
```

The `[security]` configuration sets the private key path, which is mainly used for consensus module message signing, as follows:

- `[security].private_key_path`: The private key file path, default is `conf/node.pem`.

```ini
[security]
    private_key_path=conf/node.pem
```

#### Configuring Consensus Information

Considering that PBFT module packaging too quickly can result in some blocks containing only 1 to 2 very few transactions, wasting storage space, FISCO BCOS introduces the `min_seal_time` configuration item under the variable configuration `config.ini` of `[consensus]` to control the minimum time for PBFT consensus packaging. That is: The consensus node will start the consensus process only if the packaging time exceeds `min_seal_time` and the number of packaged transactions is greater than 0, processing the newly packaged block.

- ``min_seal_time`` defaults to 500ms
- ``min_seal_time`` should not exceed the empty block time of 1000ms. If the set value exceeds 1000ms, the system defaults min_seal_time to 500ms

```ini
[consensus]
    ; min block generation time(ms)
    min_seal_time=500
```

#### Configuring Storage Information

Storage configurations are located in the `[storage]` section and include:

- `[storage].typs`: The type of database used by the blockchain node, default is RocksDB, supports TiKV. When configuring TiKV, corresponding parameters such as `pd_addrs`,`pd_ssl_ca_path`,`pd_ssl_cert_path`,`pd_ssl_key_path` need to be set;
- `[storage].data_path`: The data storage path for the blockchain node, default is `data`;
- `[storage].enable_cache`: Whether to enable caching, default is `true`;
- `[storage].key_page_size`: The storage page size in the KeyPage storage scheme, in bytes, must not be less than `4096` (4KB), default is `10240` (10KB); this configuration item can be changed to 0 to close keypage for better write performance. If an existing node changes this configuration item to 0, data cleanup and blockchain data resynchronization are required;
- `[storage].pd_addrs`: The PD address when using TiKV storage, multiple addresses are separated by commas;
- `[storage].pd_ssl_ca_path`: The PD SSL CA certificate path when using TiKV storage;
- `[storage].pd_ssl_cert_path`: The PD SSL certificate path when using TiKV storage;
- `[storage].pd_ssl_key_path`: The PD SSL private key path when using TiKV storage;
- `[storage].enable_archive`: Whether to enable archive services, default is `false`;
- `[storage].archive_ip`: The IP for the archive service to listen on;
- `[storage].archive_port`: The port for the archive service to listen on;
- `[storage].enable_separate_block_state`: When using RocksDB, whether to enable block state separation, which stores transaction receipts in a separate database for better performance, default is `false`;
- `[storage].sync_archived_blocks`: Whether to synchronize archived blocks, default is `false`. When enabled, it synchronizes historical archived blocks' transactions and receipts through P2P.

```ini
[storage]
    ; type can be tikv or rocksdb
    type=rocksdb
    data_path=data
    enable_cache=true
    ; The granularity of the storage page, in bytes, must not be less than 4096 Bytes, the default is 10240 Bytes (10KB)
    key_page_size=10240
    pd_addrs=127.0.0.1:2379
    pd_ssl_ca_path=
    pd_ssl_cert_path=
    pd_ssl_key_path=
    enable_archive=false
    archive_ip=
    archive_port=
    ;enable_separate_block_state=false
    ;sync_archived_blocks=false
```

#### Configuring Disk Encryption

Disk encryption configuration options are located in the `[storage_security]` section:

- `[storage_security].enable`: Whether to enable disk encryption, default is to disable disk encryption;
- `[storage_security].key_manager_url`: When disk encryption is enabled, `key_center_url` sets the Key Manager URL to obtain data encryption and decryption keys;
- `[storage_security].cipher_data_key`: The private key for data disk encryption.

```ini
[storage_security]
    ; enable data disk encryption or not, default is false
    enable=false
    ; url of the key center, in format of ip:port
    key_center_url=
    cipher_data_key=
```

#### Configuring Transaction Pool Information

Transaction pool configuration options are located in the `[txpool]` section:

- `[txpool].limit`: The capacity limit of the transaction pool, default is `15000`;
- `[txpool].notify_worker_num`: The number of transaction notification threads, default is 2;
- `[txpool].verify_worker_num`: The number of transaction verification threads, default is the number of CPU cores;
- `[txpool].txs_expiration_time`: The transaction expiration time in seconds, default is 10 minutes. Transactions that are not packaged by the consensus module within ten minutes will be discarded.

```ini
[txpool]
; size of the txpool, default is 15000
limit = 15000
; txs notification threads num, default is 2
notify_worker_num = 2
; txs verification threads num, default is the number of cpu cores
;verify_worker_num = 2
; txs expiration time, in seconds, default is 10 minutes
txs_expiration_time = 600
```

#### Configuring Log Information

Log configurations are primarily located in the `[log]` configuration item of `config.ini`.

- `[log].enable`: Enable/disable logging. Setting to `true` indicates that logging is enabled; setting to `false` indicates that logging is disabled. **Default is set to true. For performance testing, this option can be set to `false` to reduce the impact of printing logs on test results.**
- `[log].log_path`: The log file path.
- `[log].level`: The log level, which currently includes `trace`, `debug`, `info`, `warning`, `error`. Setting a certain log level will output logs at that level or higher. The log levels are sorted from highest to lowest as `error > warning > info > debug > trace`.
- `[log].max_log_file_size`: The maximum capacity for each log file, measured in MB, default is 200MB.
- `[log].rotate_time_point`: The log rolling time point, **default is 00:00:00**.
- `[log].format`: Configure the format for each log, with keywords wrapped in `%`, supporting keywords including `LineID, TimeStamp, ProcessID, ThreadName, ThreadID, and Message`.
- `[log].enable_rotate_by_hour`: Default is true. When set to `false`, `log.log_name_pattern,log.rotate_name_pattern,log.archive_path,log.compress_archive_file,log.max_archive_files,log.max_archive_size,log.min_free_space` take effect. Otherwise, logs generate new files by hour or file size.
- `[log].log_name_pattern`: The filename pattern for log files, which can be configured with strings or formatted characters. `%` prefix, Y, m, d, H, M, S represent year, month, day, hour, minute, second. N represents a monotonically increasing number, which can be used as `%5N` for fixed-length numbering.
- `[log].rotate_name_pattern`: The filename for the rolled log file, supporting the same formatted characters as `log.log_name_pattern`.
- `[log].archive_path`: The archive folder for historical log files.
- `[log].compress_archive_file`: Whether to compress archived log files.
- `[log].max_archive_files`: The maximum number of files in the archive folder, 0 for no limit.
- `[log].max_archive_size`: The maximum disk space limit for the archive folder, in MB, 0 for no limit.
- `[log].min_free_space`: The minimum free space for the archive folder, default is 0.

A sample log configuration is as follows:

```ini
[log]
    enable=true
    log_path=./log
    ; info debug trace
    level=DEBUG
    ; MB
    max_log_file_size=200
```

#### Gateway Module Rate Limiting

The gateway module supports configuring traffic rate limiting in `config.ini`. When traffic exceeds the limit, it achieves rate limiting by discarding data packets.

Configure the following content as needed to achieve:

- Outbound and inbound bandwidth rate limiting;
- Rate limiting for specific IPs, groups;
- Excluding specific modules from rate limiting.

The configuration in the process-dependent `config.ini` is as follows (please uncomment certain items as needed):

``` ini
[flow_control]
    ; rate limiter stat reporter interval, unit: ms
    ; stat_reporter_interval=60000

    ; time window for rate limiter, default: 3s
    ; time_window_sec=3

    ; enable distributed rate limiter, redis required, default: false
    ; enable_distributed_ratelimit=false
    ; enable local cache for distributed rate limiter, work with enable_distributed_ratelimit, default: true
    ; enable_distributed_ratelimit_cache=true
    ; distributed rate limiter local cache percent, work with enable_distributed_ratelimit_cache, default: 20
    ; distributed_ratelimit_cache_percent=20

    ; the module that does not limit bandwidth
    ; list of all modules: raft,pbft,amop,block_sync,txs_sync,light_node,cons_txs_sync
    ;
    ; modules_without_bw_limit=raft,pbft

    ; allow the msg exceed max permit pass
    ; outgoing_allow_exceed_max_permit=false

    ; restrict the outgoing bandwidth of the node
    ; both integer and decimal is support, unit: Mb
    ;
    ; total_outgoing_bw_limit=10

    ; restrict the outgoing bandwidth of the the connection
    ; both integer and decimal is support, unit: Mb
    ;
    ; conn_outgoing_bw_limit=2
    ;
    ; specify IP to limit bandwidth, format: conn_outgoing_bw_limit_x.x.x.x=n
    ;   conn_outgoing_bw_limit_192.108.0.1=3
    ;   conn_outgoing_bw_limit_192.108.0.2=3
    ;   conn_outgoing_bw_limit_192.108.0.3=3
    ;
    ; default bandwidth limit for the group
    ; group_outgoing_bw_limit=2
    ;
    ; specify group to limit bandwidth, group_outgoing_bw_limit_groupName=n
    ;   group_outgoing_bw_limit_group0=2
    ;   group_outgoing_bw_limit_group1=2
    ;   group_outgoing_bw_limit_group2=2

    ; should not change incoming_p2p_basic_msg_type_list if you known what you would to do
    ; incoming_p2p_basic_msg_type_list=
    ; the qps limit for p2p basic msg type, the msg type has been config by incoming_p2p_basic_msg_type_list, default: -1
    ; incoming_p2p_basic_msg_type_qps_limit=-1
    ; default qps limit for all module message, default: -1
    ; incoming_module_msg_type_qps_limit=-1
    ; specify module to limit qps, incoming_module_qps_limit_moduleID=n
    ;       incoming_module_qps_limit_xxxx=1000
    ;       incoming_module_qps_limit_xxxx=2000
    ;       incoming_module_qps_limit_xxxx=3000

```

### Node Consensus and Network Connection Certificates

Each node is configured with two types of certificates and their corresponding keys:

- **SSL Connection Key Certificate**: Used to identify the sender and receiver of messages in the P2P network, which includes an SSL certificate and a private key. The SSL certificate verifies the certificate chain during the handshake process to ensure it is issued by the same CA. The encryption and decryption algorithm used is RSA.

- **Consensus Key Certificate**: Primarily used to distinguish node identities within the blockchain network and to authenticate signatures when sending and receiving consensus message packages, which includes a node private key and a public key. The encryption and decryption algorithm used is ECDSA.

```bash
tree conf
conf
├── ca.crt                # CA certificate
├── cert.cnf             # Certificate settings
├── node.nodeid          # Node ID, i.e., the public key corresponding to the node private key
├── node.pem             # Node private key
├── ssl.crt              # SSL certificate
└── ssl.key               # SSL private key
```
