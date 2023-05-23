# what is eth
-------------

All or most of these components are usually combined in a single software client. For example, in Bitcoin, the reference implementation is developed by the Bitcoin Core open source project and implemented as the bitcoind client. In Ethereum, rather than a reference implementation there is a reference specification, a mathematical description of the system in the Yellow Paper (see Further Reading). There are a number of clients, which are built according to the reference specification.

## Timeline - hardforks (by block n):
-------------------------------------
Block #0
Frontier—The initial stage of Ethereum, lasting from July 30, 2015, to March 2016.

Block #200,000
Ice Age—A hard fork to introduce an exponential difficulty increase, to motivate a transition to PoS when ready.

Block #1,150,000
Homestead—The second stage of Ethereum, launched in March 2016.

Block #1,192,000
DAO—A hard fork that reimbursed victims of the hacked DAO contract and caused Ethereum and Ethereum Classic to split into two competing systems.

Block #2,463,000
Tangerine Whistle—A hard fork to change the gas calculation for certain I/O-heavy operations and to clear the accumulated state from a denial-of-service (DoS) attack that exploited the low gas cost of those operations.

Block #2,675,000
Spurious Dragon—A hard fork to address more DoS attack vectors, and another state clearing. Also, a replay attack protection mechanism.

Block #4,370,000
Metropolis Byzantium—Metropolis is the third stage of Ethereum. Launched in October 2017, Byzantium is the first part of Metropolis, adding low-level functionalities and adjusting the block reward and difficulty.

Block #7,280,000
Constantinople / St. Petersburg—Constantinople was planned to be the second part of Metropolis with similar improvements. A few hours before its activation, a critical bug was discovered. The hard fork was therefore postponed and renamed St. Petersburg.

Block #9,069,000
Istanbul—An additional hard fork with the same approach, and naming convention, as for the prior two.

Block #9,200,000
Muir Glacier—A hard fork whose sole purpose was to adjust the difficulty again due to the exponential increase introduced by Ice Age.


## components of "wEb tHrEeEeeeE"
---------------------------------

Ethereum web3.js JavaScript library, which bridges JavaScript applications that run in your browser with the Ethereum blockchain. The web3.js library also includes an interface to a P2P storage network called Swarm and a P2P messaging service called Whisper. With these three components included in a JavaScript library running in your web browser, developers have a full application development suite that allows them to build web3 DApps.


## parece una boludez pero: "cultura" eth
-----------------------------------------

In Ethereum, by comparison, the community’s development culture is focused on the future rather than the past. The (not entirely serious) mantra is "move fast and break things." If a change is needed, it is implemented, even if that means invalidating prior assumptions, breaking compatibility, or forcing clients to update. Ethereum’s development culture is characterized by rapid innovation, rapid evolution, and a willingness to deploy forward-looking improvements, even if this is at the expense of some backward compatibility.

What this means to you as a developer is that you must remain flexible and be prepared to rebuild your infrastructure as some of the underlying assumptions change. One of the big challenges facing developers in Ethereum is the inherent contradiction between deploying code to an immutable system and a development platform that is still evolving. You can’t simply "upgrade" your smart contracts. You must be prepared to deploy new ones, migrate users, apps, and funds, and start over.

