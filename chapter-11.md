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

## immediate-read

Starting with the simplest, immediate-read oracles are those that provide data that is only needed for an immediate decision, like "What is the address for ethereumbook.info?" or "Is this person over 18?" Those wishing to query this kind of data tend to do so on a "just-in-time" basis; the lookup is done when the information is needed and possibly never again. Examples of such oracles include those that hold data about or issued by organizations, such as academic certificates, dial codes, institutional memberships, airport identifiers, self-sovereign IDs, etc. This type of oracle stores data once in its contract storage, whence any other smart contract can look it up using a request call to the oracle contract. It may be updated. The data in the oracle’s storage is also available for direct lookup by blockchain-enabled (i.e., Ethereum client–connected) applications without having to go through the palaver and incurring the gas costs of issuing a transaction. A shop wanting to check the age of a customer wishing to purchase alcohol could use an oracle in this way. This type of oracle is attractive to an organization or company that might otherwise have to run and maintain servers to answer such data requests. Note that the data stored by the oracle is likely not to be the raw data that the oracle is serving, e.g., for efficiency or privacy reasons. A university might set up an oracle for the certificates of academic achievement of past students. However, storing the full details of the certificates (which could run to pages of courses taken and grades achieved) would be excessive. Instead, a hash of the certificate is sufficient. Likewise, a government might wish to put citizen IDs onto the Ethereum platform, where clearly the details included need to be kept private. Again, hashing the data (more carefully, in Merkle trees with salts) and only storing the root hash in the smart contract’s storage would be an efficient way to organize such a service.

## publish-subscribe

The next setup is publish–subscribe, where an oracle that effectively provides a broadcast service for data that is expected to change (perhaps both regularly and frequently) is either polled by a smart contract on-chain, or watched by an off-chain daemon for updates. This category has a pattern similar to RSS feeds, WebSub, and the like, where the oracle is updated with new information and a flag signals that new data is available to those who consider themselves "subscribed." Interested parties must either poll the oracle to check whether the latest information has changed, or listen for updates to oracle contracts and act when they occur. Examples include price feeds, weather information, economic or social statistics, traffic data, etc. Polling is very inefficient in the world of web servers, but not so in the peer-to-peer context of blockchain platforms: Ethereum clients have to keep up with all state changes, including changes to contract storage, so polling for data changes is a local call to a synced client. Ethereum event logs make it particularly easy for applications to look out for oracle updates, and so this pattern can in some ways even be considered a "push" service. However, if the polling is done from a smart contract, which might be necessary for some decentralized applications (e.g., where activation incentives are not possible), then significant gas expenditure may be incurred.

## request-response

The request–response category is the most complicated: this is where the data space is too huge to be stored in a smart contract and users are expected to only need a small part of the overall dataset at a time. It is also an applicable model for data provider businesses. In practical terms, such an oracle might be implemented as a system of on-chain smart contracts and off-chain infrastructure used to monitor requests and retrieve and return data. A request for data from a decentralized application would typically be an asynchronous process involving a number of steps. In this pattern, firstly, an EOA transacts with a decentralized application, resulting in an interaction with a function defined in the oracle smart contract. This function initiates the request to the oracle, with the associated arguments detailing the data requested in addition to supplementary information that might include callback functions and scheduling parameters. Once this transaction has been validated, the oracle request can be observed as an EVM event emitted by the oracle contract, or as a state change; the arguments can be retrieved and used to perform the actual query of the off-chain data source. The oracle may also require payment for processing the request, gas payment for the callback, and permissions to access the requested data. Finally, the resulting data is signed by the oracle owner, attesting to the validity of the data at a given time, and delivered in a transaction to the decentralized application that made the request—either directly or via the oracle contract. Depending on the scheduling parameters, the oracle may broadcast further transactions updating the data at regular intervals (e.g., end-of-day pricing information).

RESUMEN PORQUE LO DE ARRIBA ES UN BOLONQUI:

The steps for a request–response oracle may be summarized as follows:

Receive a query from a DApp.

Parse the query.

Check that payment and data access permissions are provided.

Retrieve relevant data from an off-chain source (and encrypt it if necessary).

Sign the transaction(s) with the data included.

Broadcast the transaction(s) to the network.

Schedule any further necessary transactions, such as notifications, etc.

## data auth (trusting the oracle)

Authenticity proofs are cryptographic guarantees that data has not been tampered with. Based on a variety of attestation techniques (e.g., digitally signed proofs), they effectively shift the trust from the data carrier to the attestor (i.e., the provider of the attestation). By verifying the authenticity proof on-chain, smart contracts are able to verify the integrity of the data before operating upon it. Oraclize is an example of an oracle service leveraging a variety of authenticity proofs.

## computation oracles

trust a centralized but (maybe) auditable off-chain service for doing a calculation that might be too mucho for evm (and it's not that unsafe maybe)

Cryplet and truebit (which looks nicer). uses miner's infra (don't know how it works with PoS)

For a more decentralized solution, we can turn to TrueBit, which offers a solution for scalable and verifiable off-chain computation. They use a system of solvers and verifiers who are incentivized to perform computations and verification of those computations, respectively. Should a solution be challenged, an iterative verification process on subsets of the computation is performed on-chain—a kind of 'verification game'. The game proceeds through a series of rounds, each recursively checking a smaller and smaller subset of the computation. The game eventually reaches a final round, where the challenge is sufficiently trivial such that the judges—Ethereum miners—can make a final ruling on whether the challenge was met, on-chain. In effect, TrueBit is an implementation of a computation market, allowing decentralized applications to pay for verifiable computation to be performed outside of the network, but relying on Ethereum to enforce the rules of the verification game. In theory, this enables trustless smart contracts to securely perform any computation task.

## decentralized oracles

chainlink might be the best example.
