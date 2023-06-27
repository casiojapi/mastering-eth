# evm

## base 

The EVM is a quasi–Turing-complete state machine; "quasi" because all execution processes are limited to a finite number of computational steps by the amount of gas available for any given smart contract execution. As such, the halting problem is "solved" (all program executions will halt) and the situation where execution might (accidentally or maliciously) run forever, thus bringing the Ethereum platform to halt in its entirety, is avoided.

The EVM has a stack-based architecture, storing all in-memory values on a stack. It works with a word size of 256 bits (mainly to facilitate native hashing and elliptic curve operations) and has several addressable data components:

An immutable program code ROM, loaded with the bytecode of the smart contract to be executed

A volatile memory, with every location explicitly initialized to zero

A permanent storage that is part of the Ethereum state, also zero-initialized

## state (cuando hablan de los tate brothers o anarcocapitalismo)

The job of the EVM is to update the Ethereum state by computing valid state transitions as a result of smart contract code execution, as defined by the Ethereum protocol. This aspect leads to the description of Ethereum as a transaction-based state machine, which reflects the fact that external actors (i.e., account holders and miners) initiate state transitions by creating, accepting, and ordering transactions. It is useful at this point to consider what constitutes the Ethereum state.

At the top level, we have the Ethereum world state. The world state is a mapping of Ethereum addresses (160-bit values) to accounts. At the lower level, each Ethereum address represents an account comprising an ether balance (stored as the number of wei owned by the account), a nonce (representing the number of transactions successfully sent from this account if it is an EOA, or the number of contracts created by it if it is a contract account), the account’s storage (which is a permanent data store, only used by smart contracts), and the account’s program code (again, only if the account is a smart contract account). An EOA will always have no code and an empty storage.

When a transaction results in smart contract code execution, an EVM is instantiated with all the information required in relation to the current block being created and the specific transaction being processed.

### out of gas (afuera de la nafta)

If at any point the gas supply is reduced to zero we get an "Out of Gas" (OOG) exception; execution immediately halts and the transaction is abandoned. No changes to the Ethereum state are applied, except for the sender’s nonce being incremented and their ether balance going down to pay the block’s beneficiary for the resources used to execute the code to the halting point



## Turing Completeness and Gas

about el castrado completo: In practical terms, if a programming language permits straight-line sequences of code, some form of if-then-else, and some form of unbounded iteration (e.g., while loops), it is Turing complete.

medio snobish pero supuestamente la evm seria quasi-turing complete porque no tiene el halting problem. O sea, que no puede estar loopeando eternamente, ya que esto se paga, y como no hay unidades infinitas de gas, i guess es imposible que pase. 

pero si, es turing complete no rompas los huevos. 

## gas

miner fee = gas cost * gas price

remaining gas = gas limit - gas cost
refunded ether = remaining gas * gas price

### negative gas costs
ethereum encourages the deletion of used storage variables and accounts by refunding some of the gas used during contract execution.
there are two operations in the evm with negative gas costs: deleting a contract (selfdestruct) is worth a refund of 24,000 gas.
changing a storage address from a nonzero value to zero (sstore[x] = 0) is worth a refund of 15,000 gas.
to avoid exploitation of the refund mechanism, the maximum refund for a transaction is set to half the total amount of gas used (rounded down).

## block gas limit
The block gas limit is the maximum amount of gas that may be consumed by all the transactions in a block, and constrains how many transactions can fit into a block.

For example, let’s say we have 5 transactions whose gas limits have been set to 30,000, 30,000, 40,000, 50,000, and 50,000. If the block gas limit is 180,000, then any four of those transactions can fit in a block, while the fifth will have to wait for a future block. As previously discussed, miners decide which transactions to include in a block. Different miners are likely to select different combinations, mainly because they receive transactions from the network in a different order.

OJO con las combinetas que se pueden hacer para maxear gas. 

### que onda con el block gas limit

The miners on the network collectively decide the block gas limit. Individuals who want to mine on the Ethereum network use a mining program, such as Ethminer, which connects to a Geth or Parity Ethereum client. The Ethereum protocol has a built-in mechanism where miners can vote on the gas limit so capacity can be increased or decreased in subsequent blocks. The miner of a block can vote to adjust the block gas limit by a factor of 1/1,024 (0.0976%) in either direction. The result of this is an adjustable block size based on the needs of the network at the time. This mechanism is coupled with a default mining strategy where miners vote on a gas limit that is at least 4.7 million gas, but which targets a value of 150% of the average of recent total gas usage per block (using a 1,024-block exponential moving average). 

eth2 como funca esto???????
