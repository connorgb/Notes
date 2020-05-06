# Ethereum

## Overview
* Ethereum is a deterministic but unbounded state machine consisting of a globally accessible singleton state and a virtual machine that applies changes to that state
* It executes programs called smart contracts, and uses a blockchain to synchronize and store the system’s state changes 
* Ether is a currency used to meter the resource costs
* Ethereum is designed to be a blockchain that runs a virtual machine
Conceived as a piece of a suite of decentralized technologies, the other two being whisper and swarm
* See Vitalik Buterin’s ‘A Prehistory of Ethereum’
* ### Ethereum’s Components
  * P2P network 
    * The ethereum main network, addressable on TCP port 30303, running the DEVp2p
  * Consensus rules 
    * Ethereum’s consensus rules are defined in the reference specifications: ‘the yellow paper’
  * Transactions
    * Ethereum transactions are network messages that include (among other things) a sender, recipient, value, and data payload
  * State machine
    * State transitions are processed by the Ethereum Virtual Machine, a stack-based virtual machine that executes bytecode. ‘Smart contracts’ are written in high-level languages and compiled to bytecode
  * Data structures 
    * Ethereum’s state is stored locally on each node as a database (usually Google’s levelDB), which contains the transactions and system state in a serialized hashed data structure called a Merkle Patricia Tree
  * Consensus algorithm
    * Ethereum uses Bitcoins consensus model, Nakamoto Consensus, which uses sequential single-signature blocks, weighted in importance by PoW (proof of work) to determine the longest chain and therefore the current state. There are plans to move to a PoS (proof of stake) weighted voting system called Casper
  * Economic security 
    * Ethereum currently uses a PoW algorithm called Ethash, but will be switched to a PoS system
  * Clients
    * Several interoperable implementations, like Geth and Parity
* Further reading:
  * DEVp2p protocol
  * Leveldb.org
* Turing completeness
  * Because ethereum processes programs (smart contracts) on each node, it must have some way to prevent it from processing infinite transactions (effectively a ddos), and also to promote efficient transactions. To this end it uses gas as a meter of the instructions in a smart contract. Gas is purchased with ether at the time of the transaction
* Decentralized Apps
  * A DApp is a web interface of a smart contract, built on decentralized, p2p infrastructure
  * It can also include storage (swarm) and messaging (whisper) protocols using web3.js
* Web3
  * Represents applications built on decentralized protocols
* Development culture
  * “Move fast and break things”
  * Be prepared to rebuild infrastructure
    * How does one deploy code to an ‘immutable system’ on an evolving development platform?
    * Can’t simply upgrade, prepare to start over
  * Too unstable for ‘decentralized autonomy’ quite yet
    * In order to restart smart contracts, you have to retain control over them

## Basics
* Ether is divided into wei: 1 wei is 1 quintillion ether (1/1000000000000000000th)
  * Ether value is encoded/represented in an unsigned int of wei
* 2 types of accounts: Externally owned accounts and Contracts
  * EOAs have private keys, Contracts have smart contract codes and are controlled by that software, which is executed on the EVM
  * Contracts cannot initiate transactions. EOAs initiate, contracts react
* Solidity faucet example (code/solidity/Faucet.sol found at github.com/ethereumbook/ethereumbook/):
```
// A faucet contract!
contract faucet {
	function withdraw(uint withdraw_amount) public {
		require(withdraw_amount <= 100000000000000000);
		msg.sender.transfer(withdraw_amount);
	}
	function () public payable {}
}
// End of program
```
* First it declares a contract object, similar to a class declaration. Then it declares a function which takes an unsigned int argument, and is public. It throws an exception if the withdrawal is greater than .1 eth. Then it transfers the amount to the sender of the msg that triggered this contract. `public payable` is the default function, called if the transaction that triggered the contract did not name any functions. It's used to accept ether. 



## Clients
* Ethereum clients are applications that implement the ethereum protocol and communicate over the P2P network with other clients
  * Clients’ code is available under open source licenses
* Bitcoin’s spec is the reference implementation of Bitcoin Core, Ethereum’s specification is formalised mathematical and written and defines standard behaviour
  * Because it is formalised, there are several interoperable software implementations (clients)
* There are a number of ethereum-based networks (blockchains) that are mostly compatible at the protocol level, they do require some maintenance on the part of clients to support each network
* Harmony is the Java client
* The strength of the blockchain depends on them having many independent nodes
  * Nodes can help other nodes bootstrap, and offer independent verification
* Nodes must download and store a great deal of data, and this amount is increasing rapidly
  * Full nodes aren’t necessary for development. You can use a testnet, a local private blockchain, or a cloud-based ethereum client
  * You can also run a remote client which can create and broadcast transactions
  * ‘Wallet’ and ‘remote client’ are used interchangeably, though they differ in that remote clients offer APIs like web3.js
    * Remote wallets also differ from light clients, which validate block headers and use merkle proofs to validate transactions, similar to full nodes
    * Remote clients trust a full node to access the blockchain, and sacrifice security and anonymity
* Full nodes 
  * can interact with any contract on the blockchain, and deploy contracts, and query the blockchain offline and anonymously
  * Incur significant bandwidth and memory costs and need constant maintenance and upgrades
* Public testnets 
  * Sync and store much less data than full nodes. Contracts and transactions are free, while still being live and public blockchains
  * They cannot be used to test for real adversaries since it’s not real money, and they cannot test transaction fees or network congestion
* Local blockchain simulations (single instance private blockchains)
  * Require almost no syncing or data; mining is trivial and you are the only user. There is no sequencing of transactions and mining is predictable
  * You have to deploy everything you want to test, including dependencies and libraries. You also cannot recreate some public contracts like DAOs
* To run a full node you need at least 8Gb of ram and an SSD or you will not be able to keep up and sync fully
* We assume you are using ubuntu bash shell
