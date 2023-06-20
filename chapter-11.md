# oracles (tipo java) ((ahre))

## why oracles?

This has two particularly important consequences: the first is that there can be no intrinsic source of randomness for the EVM and smart contracts to work with; the second is that extrinsic data can only be introduced as the data payload of a transaction.

Let’s unpack those two consequences further. To understand the prohibition of a true random function in the EVM to provide randomness for smart contracts, consider the effect on attempts to achieve consensus after the execution of such a function: node A would execute the command and store 3 on behalf of the smart contract in its storage, while node B, executing the same smart contract, would store 7 instead. Thus, nodes A and B would come to different conclusions about what the resulting state should be, despite having run exactly the same code in the same context. Indeed, it could be that a different resulting state would be achieved every time that the smart contract is evaluated. As such, there would be no way for the network, with its multitude of nodes running independently around the world, to ever come to a decentralized consensus on what the resulting state should be. In practice, it would get much worse than this example very quickly, because knock-on effects, including ether transfers, would build up exponentially.

## use cases

Oracles can therefore be thought of as a mechanism for bridging the gap between the off-chain world and smart contracts.

## oracle design 

Oracle Design Patterns
All oracles provide a few key functions, by definition. These include the ability to:

Collect data from an off-chain source.

Transfer the data on-chain with a signed message.

Make the data available by putting it in a smart contract’s storage.

Once the data is available in a smart contract’s storage, it can be accessed by other smart contracts via message calls that invoke a "retrieve" function of the oracle’s smart contract; it can also be accessed by Ethereum nodes or network-enabled clients directly by "looking into” the oracle’s storage.

The three main ways to set up an oracle can be categorized as request–response, publish-subscribe, and immediate-read.

## oracle design 

Oracle Design Patterns
All oracles provide a few key functions, by definition. These include the ability to:

Collect data from an off-chain source.

Transfer the data on-chain with a signed message.

Make the data available by putting it in a smart contract’s storage.

Once the data is available in a smart contract’s storage, it can be accessed by other smart contracts via message calls that invoke a "retrieve" function of the oracle’s smart contract; it can also be accessed by Ethereum nodes or network-enabled clients directly by "looking into” the oracle’s storage.

The three main ways to set up an oracle can be categorized as request–response, publish-subscribe, and immediate-read.


