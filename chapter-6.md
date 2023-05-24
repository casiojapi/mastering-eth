# chapter 6: transactions
-------------------------

## transactions
Transactions are signed messages originated by an externally owned account (EOA), transmitted by the Ethereum network, and recorded on the Ethereum blockchain.

tx are the only thing that can trigger a change of state or cause a contract to excecute in the evm. contracts don't run on their own. everything happens with a tx.

## structure of a tx
a tx is a serialized bin message that contains the following data:
+ nonce: sequece number - used to prevent message replay.
+ gas price: amount of ether (in wei) that the originator (or sender?) is willing to pay for each unit of gas
+ gas limit: the maximum amount of gas the originator is willing to buy for this tx
+ recipient: destination (eth address)
+ value: amount of ether (in wei) to send to destination
+ data: binary data payload
+ v,r,s: the 3 components of an ECDSA digital signature of the origin EOA (externally owned account)

all this data is serialized using RLP (recursive length prefix, created for data serialization in ethereum.
numbers are encoded as big endians ints, multiples of 8 bits

## transaction nonce
nonce: A scalar value equal to the number of transactions sent from this address or, in the case of accounts with associated code, the number of contract-creations made by this account.

sorry for the copy but its a great example of the use of nonce:

Imagine you wish to make two transactions. You have an important payment to make of 6 ether, and also another payment of 8 ether. You sign and broadcast the 6-ether transaction first, because it is the more important one, and then you sign and broadcast the second, 8-ether transaction. Sadly, you have overlooked the fact that your account contains only 10 ether, so the network can’t accept both transactions: one of them will fail. Because you sent the more important 6-ether one first, you understandably expect that one to go through and the 8-ether one to be rejected. However, in a decentralized system like Ethereum, nodes may receive the transactions in either order; there is no guarantee that a particular node will have one transaction propagated to it before the other. As such, it will almost certainly be the case that some nodes receive the 6-ether transaction first and others receive the 8-ether transaction first. Without the nonce, it would be random as to which one gets accepted and which rejected. However, with the nonce included, the first transaction you sent will have a nonce of, let’s say, 3, while the 8-ether transaction has the next nonce value (i.e., 4). So, that transaction will be ignored until the transactions with nonces from 0 to 3 have been processed, even if it is received first.

another security example for the use of nonce (different from the nonce in btc): 

Now imagine you have an account with 100 ether. Fantastic! You find someone online who will accept payment in ether for a mcguffin-widget that you really want to buy. You send them 2 ether and they send you the mcguffin-widget. Lovely. To make that 2-ether payment, you signed a transaction sending 2 ether from your account to their account, and then broadcast it to the Ethereum network to be verified and included on the blockchain. Now, without a nonce value in the transaction, a second transaction sending 2 ether to the same address a second time will look exactly the same as the first transaction. This means that anyone who sees your transaction on the Ethereum network (which means everyone, including the recipient or your enemies) can "replay" the transaction again and again and again until all your ether is gone simply by copying and pasting your original transaction and resending it to the network. However, with the nonce value included in the transaction data, every single transaction is unique, even when sending the same amount of ether to the same recipient address multiple times. Thus, by having the incrementing nonce as part of the transaction, it is simply not possible for anyone to "duplicate" a payment you have made.

## gaps in nonces, duplicates and confirmation

The Ethereum network processes transactions sequentially, based on the nonce. That means that if you transmit a transaction with nonce 0 and then transmit a transaction with nonce 2, the second transaction will not be included in any blocks. It will be stored in the mempool, while the Ethereum network waits for the missing nonce to appear. All nodes will assume that the missing nonce has simply been delayed and that the transaction with nonce 2 was received out of sequence.

You should be equally mindful that once a transaction with the "missing" nonce is validated by the network, all the broadcast transactions with subsequent nonces will incrementally become valid; it is not possible to "recall" a transaction!

If you duplicate a nonce, for example by transmitting two transactions with the same nonce but different recipients or values, then one of them will be confirmed and one will be rejected. Which one is confirmed will be determined by the sequence in which they arrive at the first validating node that receives them—i.e., it will be fairly random.

## concurrency with nonces

basically don't sign concurrently. signature depends on the nonce, and if you are setting nonces concurrently, shit will get wild. 

## transaction recipient

The recipient of a transaction is specified in the to field. This contains a 20-byte Ethereum address. The address can be an EOA or a contract address.

## transaction value and data

A transaction with only value is a payment. A transaction with only data is an invocation. A transaction with both value and data is both a payment and an invocation. A transaction with neither value nor data—well that’s probably just a waste of gas! But it is still possible.

## Transmitting a Data Payload to an EOA or Contract

When your transaction contains data, it is most likely addressed to a contract address. That doesn’t mean you cannot send a data payload to an EOA—that is completely valid in the Ethereum protocol. However, in that case, the interpretation of the data is up to the wallet you use to access the EOA. It is ignored by the Ethereum protocol. Most wallets also ignore any data received in a transaction to an EOA they control. In the future, it is possible that standards may emerge that allow wallets to interpret data the way contracts do, thereby allowing transactions to invoke functions running inside user wallets. The critical difference is that any interpretation of the data payload by an EOA is not subject to Ethereum’s consensus rules, unlike a contract execution.


The data payload sent to an ABI-compatible contract (which you can assume all contracts are) is a hex-serialized encoding of:

+ A function selector
The first 4 bytes of the Keccak-256 hash of the function’s prototype. This allows the contract to unambiguously identify which function you wish to invoke.

+ The function arguments
The function’s arguments, encoded according to the rules for the various elementary types defined in the ABI specification.

## contract creation

Contract creation transactions are sent to a special destination address called the zero address; the to field in a contract registration transaction contains the address 0x0. This address represents neither an EOA (there is no corresponding private–public key pair) nor a contract. It can never spend ether or initiate a transaction. It is only used as a destination, with the special meaning "create this contract."

## digital signatures

ECDSA.

basic: A digital signature serves three purposes in Ethereum (see the following sidebar). First, the signature proves that the owner of the private key, who is by implication the owner of an Ethereum account, has authorized the spending of ether, or execution of a contract. Secondly, it guarantees non-repudiation: the proof of authorization is undeniable. Thirdly, the signature proves that the transaction data has not been and cannot be modified by anyone after the transaction has been signed.

## Transaction Signing in Practice

To produce a valid transaction, the originator must digitally sign the message, using the Elliptic Curve Digital Signature Algorithm. When we say "sign the transaction" we actually mean "sign the Keccak-256 hash of the RLP-serialized transaction data." The signature is applied to the hash of the transaction data, not the transaction itself.

To sign a transaction in Ethereum, the originator must:

Create a transaction data structure, containing nine fields: nonce, gasPrice, gasLimit, to, value, data, chainID, 0, 0.

Produce an RLP-encoded serialized message of the transaction data structure.

Compute the Keccak-256 hash of this serialized message.

Compute the ECDSA signature, signing the hash with the originating EOA’s private key.

Append the ECDSA signature’s computed v, r, and s values to the transaction.

## v, r, s

v, r, s are the values for the transaction's signature. They can be used as in Get public key of any ethereum account

A little more information, r and s are outputs of an ECDSA signature, and v is the recovery id. For replay attack prevention, Ethereum makes further adjustments to v as explained in EIP 155.

### about v adjustments and EIP 155

At block #2,675,000 (nov 2016) Ethereum implemented the "Spurious Dragon" hard fork, which, among other changes, introduced a new signing scheme that includes transaction replay protection (preventing transactions meant for one network being replayed on others). This new signing scheme is specified in EIP-155. This change affects the form of the transaction and its signature, so attention must be paid to the first of the three signature variables (i.e., v), which takes one of two forms and indicates the data fields included in the transaction message being hashed.

## there are no native multisig accounts. 

but you can use a contract to set the conditions you want for spending. But its not the same as btc.
