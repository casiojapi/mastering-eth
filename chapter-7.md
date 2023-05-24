# chapter 7: smart contracts and solidity

 there are two different types of accounts in Ethereum: externally owned accounts (EOAs) and contract accounts. EOAs are controlled by users, often via software such as a wallet application that is external to the Ethereum platform. In contrast, contract accounts are controlled by program code (also commonly referred to as “smart contracts”) that is executed by the Ethereum Virtual Machine. In short, EOAs are simple accounts without any associated code or data storage, whereas contract accounts have both associated code and data storage.

## what is a smart contract?

smart contracts more like immutable computer programs that run deterministically in the context of an Ethereum Virtual Machine as part of the Ethereum network protocol


## life cycle of a smart contract

smart contracts are typically written in a high-level language, such as solidity. but in order to run, they must be compiled to the low-level bytecode that runs in the evm. once compiled, they are deployed on the ethereum platform using a special contract creation transaction, which is identified as such by being sent to the special contract creation address, namely 0x0. each contract is identified by an ethereum address, which is derived from the contract creation transaction as a function of the originating account and nonce. the ethereum address of a contract can be used in a transaction as the recipient, sending funds to the contract or calling one of the contract’s functions. note that, unlike with eoas, there are no keys associated with an account created for a new smart contract. as the contract creator, you don’t get any special privileges at the protocol level (although you can explicitly code them into the smart contract). you certainly don’t receive the private key for the contract account, which in fact does not exist—we can say that smart contract accounts own themselves.


## introduction to ethereum high-level languages

the ethereum contract abi

gas considerations

conclusions
