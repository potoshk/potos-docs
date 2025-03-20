# EVM engine

The Ethereum Virtual Machine (EVM) is a decentralized virtual environment that executes code consistently and securely across all Ethereum nodes.

FISCO BCOS embraces the Ethereum development ecosystem, fully supporting the Solidity contract language up to version 0.8.26.

FISCO BCOS uses the EVM implementation called [evmone](https://github.com/ethereum/evmone), which is a C++ implementation and is the high-performing EVM implementation.

## EVMC interface usage

The EVMC interface is used to connect the EVM implementation to the FISCO BCOS node. The EVMC interface is a standard interface that allows the FISCO BCOS node to support multiple EVM implementations.

EVMC defines two main types of invocation interfaces:

- **Instance interface:** the interface through which the node invokes the EVM
- **Callback interface:** interface of EVM callback to node

The EVM itself does not save state data. The node operates the EVM through the instance interface. The EVM, in turn, adjusts the Callback interface to operate the state of the node.

![](../_static/glossary/evmc.png)

## EVM execution process

`Solidity` is the execution language for smart contracts, and once compiled by the `Solidity compiler (solc)`, it transforms into EVM instructions that are akin to assembly language. The EVM Interpreter defines a comprehensive set of these instructions. After compilation, Solidity generates a binary file, which is a collection of EVM instructions. Transactions are sent to nodes in binary form, and upon receipt, nodes invoke the EVM to execute these instructions through EVMC. Within the EVM, the logic of these instructions is simulated and implemented through code.

Solidity is a stack-based language, and the EVM also operates on a stack when executing binary code. This stack-based architecture allows for an efficient and organized way to manage operations and data flow within the EVM environment.

## Arithmetic instruction example

An `ADD` instruction within the EVM is implemented as follows. The stack pointer (SP) refers to the top of the stack. The `ADD` instruction retrieves data from the first and second positions on the stack (SP[0], SP[1]), adds them together, and then writes the result back to the top of the stack (SPP[0]).

```c
CASE(ADD)
{
    ON_OP();
    updateIOGas();

    // Pops two items and pushes their sum mod 2^256.
    m_SPP[0] = m_SP[0] + m_SP[1];
}
```

## Jump instruction example

The `JUMP` instruction facilitates jumps between bytecode. It first retrieves the address to jump to from the top of the stack (SP[0]), verifies that it is within bounds, places it in the program counter (PC), and the next instruction begins execution from the location pointed to by PC.

```c
CASE(JUMP)
{
    ON_OP();
    updateIOGas();
    m_PC = verifyJumpDest(m_SP[0]);
}
```

State Read Instruction Example

`SLOAD` is used to query state data. The general process involves retrieving a key from the top of the stack (SP[0]), using this key as an argument, and then calling the `get_storage()` callback function from the EVMC to query the corresponding value for that key. The retrieved value is then written to the top of the result stack (SPP[0]).

```c
CASE(SLOAD)
{
    m_runGas = m_rev >= EVMC_TANGERINE_WHISTLE ? 200 : 50;
    ON_OP();
    updateIOGas();

    evmc_uint256be key = toEvmC(m_SP[0]);
    evmc_uint256be value;
    m_context->fn_table->get_storage(&value, m_context, &m_message->destination, &key);
    m_SPP[0] = fromEvmC(value);
}
```

## State write instruction example

The `SSTORE` instruction writes data to the node's state. The general process involves retrieving a key and value from the first and second positions on the stack (SP[0], SP[1]), using these as arguments, and calling the `set_storage()` callback function from the EVMC to write to the node's state.

```c
CASE(SSTORE)
{
    ON_OP();
    if (m_message->flags & EVMC_STATIC)
        throwDisallowedStateChange();

    static_assert(
        VMSchedule::sstoreResetGas <= VMSchedule::sstoreSetGas, "Wrong SSTORE gas costs");
    m_runGas = VMSchedule::sstoreResetGas;  // Charge the modification cost up front.
    updateIOGas();

    evmc_uint256be key = toEvmC(m_SP[0]);
    evmc_uint256be value = toEvmC(m_SP[1]);
    auto status =
        m_context->fn_table->set_storage(m_context, &m_message->destination, &key, &value);

    if (status == EVMC_STORAGE_ADDED)
    {
        // Charge additional amount for added storage item.
        m_runGas = VMSchedule::sstoreSetGas - VMSchedule::sstoreResetGas;
        updateIOGas();
    }
}
```

## Contract call instruction example

The `CALL` instruction enables the invocation of another contract based on its address. Initially, the EVM identifies the `CALL` instruction and calls `caseCall()`. Within `caseCall()`, `caseCallSetup()` retrieves data from the stack, encapsulates it into a message (`msg`), and uses this as an argument to call the `call` callback function from the EVMC. After being called back with `call()`, a new EVM is initiated to handle the invocation. The execution results from the new EVM are then returned to the current EVM via the `call()` parameters, and the current EVM writes the results to the result stack (SSP), concluding the invocation. The logic for contract creation is similar.

```c
CASE(CALL)
CASE(CALLCODE)
{
    ON_OP();
    if (m_OP == Instruction::DELEGATECALL && m_rev < EVMC_HOMESTEAD)
        throwBadInstruction();
    if (m_OP == Instruction::STATICCALL && m_rev < EVMC_BYZANTIUM)
        throwBadInstruction();
    if (m_OP == Instruction::CALL && m_message->flags & EVMC_STATIC && m_SP[2] != 0)
        throwDisallowedStateChange();
    m_bounce = &VM::caseCall;
}
BREAK

void VM::caseCall()
{
    m_bounce = &VM::interpretCases;

    evmc_message msg = {};

    // Clear the return data buffer. This will not free the memory.
    m_returnData.clear();

    bytesRef output;
    if (caseCallSetup(msg, output))
    {
        evmc_result result;
        m_context->fn_table->call(&result, m_context, &msg);

        m_returnData.assign(result.output_data, result.output_data + result.output_size);
        bytesConstRef{&m_returnData}.copyTo(output);

        m_SPP[0] = result.status_code == EVMC_SUCCESS ? 1 : 0;
        m_io_gas += result.gas_left;

        if (result.release)
            result.release(&result);
    }
    else
    {
        m_SPP[0] = 0;
        m_io_gas += msg.gas;
    }
    ++m_PC;
}
```

## Summary

The EVM is a state-executing machine that takes as input the binary instructions compiled from Solidity and the node's state data, and outputs changes to the node's state. Ethereum has achieved compatibility with various virtual machines through the EVMC.
