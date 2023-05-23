# chapter 2: what is
--------------------


The EVM is a global singleton, meaning that it operates as if it were a global, single-instance computer, running everywhere. Each node on the Ethereum network runs a local copy of the EVM to validate contract execution, while the Ethereum blockchain records the changing state of this world computer as it processes transactions and smart contracts.


## EOAS (externally owned accounts) and contract accounts

EOA: wallet is called an externally owned account (EOA). Externally owned accounts are those that have a private key; having the private key means control over access to funds or contracts. 

contract account: has smart contract code, which a simple EOA can’t have. Furthermore, a contract account does not have a private key. Instead, it is owned (and controlled) by the logic of its smart contract code: the software program recorded on the Ethereum blockchain at the contract account’s creation and executed by the EVM.
