# chapter 7: smart contracts and solidity

 there are two different types of accounts in Ethereum: externally owned accounts (EOAs) and contract accounts. EOAs are controlled by users, often via software such as a wallet application that is external to the Ethereum platform. In contrast, contract accounts are controlled by program code (also commonly referred to as “smart contracts”) that is executed by the Ethereum Virtual Machine. In short, EOAs are simple accounts without any associated code or data storage, whereas contract accounts have both associated code and data storage.

## what is a smart contract?

smart contracts more like immutable computer programs that run deterministically in the context of an Ethereum Virtual Machine as part of the Ethereum network protocol


## life cycle of a smart contract

smart contracts are typically written in a high-level language, such as solidity. but in order to run, they must be compiled to the low-level bytecode that runs in the evm. once compiled, they are deployed on the ethereum platform using a special contract creation transaction, which is identified as such by being sent to the special contract creation address, namely 0x0. each contract is identified by an ethereum address, which is derived from the contract creation transaction as a function of the originating account and nonce. the ethereum address of a contract can be used in a transaction as the recipient, sending funds to the contract or calling one of the contract’s functions. note that, unlike with eoas, there are no keys associated with an account created for a new smart contract. as the contract creator, you don’t get any special privileges at the protocol level (although you can explicitly code them into the smart contract). you certainly don’t receive the private key for the contract account, which in fact does not exist—we can say that smart contract accounts own themselves.

selfdestruct

To delete a contract, you execute an EVM opcode called SELFDESTRUCT (previously called SUICIDE). That operation costs “negative gas,” a gas refund, thereby incentivizing the release of network client resources from the deletion of stored state. Deleting a contract in this way does not remove the transaction history (past) of the contract, since the blockchain itself is immutable. It is also important to note that the SELFDESTRUCT capability will only be available if the contract author programmed the smart contract to have that functionality. If the contract’s code does not have a SELFDESTRUCT opcode, or it is inaccessible, the smart contract cannot be deleted.

## introduction to ethereum high-level languages

Serpent
A procedural (imperative) programming language with a syntax similar to Python. Can also be used to write functional (declarative) code, though it is not entirely free of side effects.

Solidity
A procedural (imperative) programming language with a syntax similar to JavaScript, C++, or Java. The most popular and frequently used language for Ethereum smart contracts.

Vyper
A more recently developed language, similar to Serpent and again with Python-like syntax. Intended to get closer to a pure-functional Python-like language than Serpent, but not to replace Serpent.

Bamboo
A newly developed language, influenced by Erlang, with explicit state transitions and without iterative flows (loops). Intended to reduce side effects and increase auditability. Very new and yet to be widely adopted.

## the ethereum contract abi

The ABI is thus the primary way of encoding and decoding data into and out of machine code.

In Ethereum, the ABI is used to encode contract calls for the EVM and to read data out of transactions. The purpose of an ABI is to define the functions in the contract that can be invoked and describe how each function will accept arguments and return its result.

A contract’s ABI is specified as a JSON array of function descriptions (see Functions) and events (see Events). A function description is a JSON object with fields type, name, inputs, outputs, constant, and payable. An event description object has fields type, name, inputs, and anonymous.

We use the solc command-line Solidity compiler to produce the ABI for our Faucet.sol example contract:

$ solc --abi Faucet.sol
======= Faucet.sol:Faucet =======
Contract JSON ABI
[{"inputs":[{"internalType":"uint256","name":"withdraw_amount","type":"uint256"}], \
"name":"withdraw","outputs":[],"stateMutability":"nonpayable","type":"function"}, \
{"stateMutability":"payable","type":"receive"}]

## gas considerations

Avoid Dynamically Sized Arrays
Any loop through a dynamically sized array where a function performs operations on each element or searches for a particular element introduces the risk of using too much gas. Indeed, the contract may run out of gas before finding the desired result, or before acting on every element, thus wasting time and ether without giving any result at all.

Avoid Calls to Other Contracts
Calling other contracts, especially when the gas cost of their functions is not known, introduces the risk of running out of gas. Avoid using libraries that are not well tested and broadly used. The less scrutiny a library has received from other programmers, the greater the risk of using it.

Estimating Gas Cost
If you need to estimate the gas necessary to execute a certain method of a contract considering its arguments, you could use the following procedure:

var contract = web3.eth.contract(abi).at(address);
var gasEstimate = contract.myAweSomeMethod.estimateGas(arg1, arg2,
    {from: account});
gasEstimate will tell you the number of gas units needed for its execution. It is an estimate because of the Turing completeness of the EVM—it is relatively trivial to create a function that will take vastly different amounts of gas to execute different calls. Even production code can change execution paths in subtle ways, resulting in hugely different gas costs from one call to the next. However, most functions are sensible and estimateGas will give a good estimate most of the time.

To obtain the gas price from the network you can use:

var gasPrice = web3.eth.getGasPrice();
And from there you can estimate the gas cost:

var gasCostInEther = web3.utils.fromWei((gasEstimate * gasPrice), 'ether');

