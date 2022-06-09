- [Blockchain Basics](#blockchain-basics)
- [What is 'gas'?](#what-is-gas)
- [Ethereum Structure](#ethereum-structure)
  - [What's in a block?](#whats-in-a-block)
- [Ethereum Operations](#ethereum-operations)
- [EOA vs Smart contract](#eoa-vs-smart-contract)
- [IoT and Ethereum](#iot-and-ethereum)
- [UTXO](#utxo)
- [Algorithms & Techniques: Public-Key Cryptography](#algorithms--techniques-public-key-cryptography)
  - [Public-key cryptography](#public-key-cryptography)
  - [Hashing](#hashing)
  - [Transaction integrity](#transaction-integrity)
  - [Securing Blockchain](#securing-blockchain)
- [Decentralized Systems](#decentralized-systems)
  - [Trust Train](#trust-train)
- [Robustness](#robustness)
  - [When more than one miner solves the consensus puzzle](#when-more-than-one-miner-solves-the-consensus-puzzle)
    - [Chain Split](#chain-split)
  - [Double spending](#double-spending)
- [Forks](#forks)
  - [Hard Fork](#hard-fork)
  - [Soft Fork](#soft-fork)

# Blockchain Basics

+ how proven algorithms/techniques for encryption, hashing and peer to peer networks have been creatively applied to the innovation of blockchain, a decentralized, trusted, distributed, immutable ledger

# What is 'gas'?

+ execution fee for every operation made on ethereum
+ special unit used in Ethereum
+ measures how much "work" an action or set of actions takes to perform
+ Every operation that can be performed by a transaction or contract on the Ethereum platform costs a certain number of gas, with operations that require more computational resources costing more gas than operations that require few computational resources
+ price is expressed in ether and it's decided by the miners, which can refuse to process transaction with less than a certain gas price
+ helps to ensure an appropriate fee is being paid by transactions submitted to the network
  + ensure that network doesn't become bogged down with performing a lot of intensive work that isn't valuable to anyone
  + different strategy than the Bitcoin transaction fee, which is based only on the size in kilobytes of a transaction
  +  important to measure the work done directly instead of just choosing a fee based on the length of a transaction or contract(like Bitcoin)
+ isn't any actual token for gas
  + exists only inside of the Ethereum virtual machine as a count of how much work is being performed
  + charged as a certain number of ether
  + built-in token on the Ethereum network and the token with which miners are rewarded for producing blocks
+ helpful to separate out the price of computation from the price of the ether token, so that the cost of an operation doesn't have to be changed every time the market moves

---

+ Operations in the EVM have gas cost, but gas itself also has a gas price measured in terms of ether
  + there is a difference between your transaction running out of gas and your transaction not having a high enough fee
  + gas price I set in my transaction is too low, no one will even bother to run my transaction in the first place
+ if I provide an acceptable gas price, and then my transaction results in so much computational work that the combined gas costs go past the amount I attached as a fee, that gas counts as "spent" and I don't get it back
  + miner will stop processing the transaction, revert any changes it made, but still include it in the blockchain as a "failed transaction", collecting the fees for it
+ If you set a very high gas price, you will end up paying lots of ether for only a few operations
  + If you provided a normal gas price, however, and just attached more ether than was needed to pay for the gas that your transaction consumed, the excess amount will be refunded back to you
+ You can think of the gas price as the hourly wage for the miner, and the gas cost as their timesheet of work performed

---

+ Gas is the way that fees are calculated
+ The fees are still paid in ether, though, which is different from gas
+ The gas cost is the amount of work that goes into something, like the number of hours of labour, whereas the gas price is like the hourly wage you pay for the work to be done. The combination of the two determines your total transaction fee.
+ If your gas price is too low, no one will process your transaction
+ If your gas price is fine but the gas cost of your transaction runs "over budget" the transaction fails but still goes into the blockchain, and you don't get the money back for the work that the labourers did.
+ This makes sure that nothing runs forever, and that people will be careful about the code that they run. It keeps both miners and users safe from bad code

---

# Ethereum Structure

<img src="https://ethereum.org/static/85d784391401f89209d3bcc51e0ea677/302a4/tx-block.png" width=500>

## What's in a block?

+ timestamp – the time when the block was mined.
+ blockNumber – the length of the blockchain in blocks.
+ baseFeePerGas - the minimum fee per gas required for a transaction to be included in the block.
+ difficulty – the effort required to mine the block.
+ mixHash – a unique identifier for that block.
+ parentHash – the unique identifier for the block that came before (this is how blocks are linked in a chain).
+ transactions – the transactions included in the block.
+ stateRoot – the entire state of the system: account balances, contract storage, contract code and account nonces are inside.
+ nonce – a hash that proves that the block has gone through proof-of-work when combined with the mixHash

---

# Ethereum Operations

+ an Ethereum node: a computational system representing a business entity or an individual participant
+ an Ethereum full node: hosts the software needed for transaction initiation, validatoin, mining, block creation, smart contract execution and EVM
+ smart contract is designed, developed, compiled and deployed on the EVM that can be more than one smart contract in an EVM
  + when target address in transaction is a smart contract, the execution conde corresponding to the smart contract is activated and executed on the EVM
  + input needed for the execution is extracted from the payload field of the transaction
  + current state of smart contract is the values of the variables defined in it
  + sate of smart contract may be updated by this execution
  + results of execution is told in the receipts
+ all transactions generated are validated
  + checking time-stamp & nonce combination to be valid & availability of sufficient fees for execution
  + miner nodes receive, verify, gather, execute transactions
+ validated transactions are broadcasted and gathered for block creation

---

# EOA vs Smart contract

||EOA (account)|Contract account (Smart contract)|
|---|---|---|
|Creation|created at any time, using wallet generation utility|can only be created from an EOA or from an existing deployed contract|
|Public Address|derived from its private key|created by the combination of a public address of creating account + nonce|
|Private keys|can only be controlled using its private key|controlled by the contract|

---

# IoT and Ethereum

+ connected to Internet &rarr; threats of hacking, data forgery & theft
+ prevent by basic security & transparency & decentralization &rarr; security & integrity
+ each device builds data chain
+ unique ID assigned to each core data makes real-time data tracking transparent
+ distributed storage(분산 스토리지) increases safety of data management

---

# UTXO

: Unspent Transaction Output 쓰지 않은 잔액

+ each wallet's UTXOs are locked by public key of wallet's owner

**Ex.**

Input: 5 BTC, Output: 4.9 BTC, 0.1 BTC

&rarr; 4.9 BTC &rarr; UTXO of receiver

&rarr; 0.1 BTC &rarr; UTXO of miner

**Ex.**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F9999A93D5B57247A24FF5D" width=400>

+ When G - (9 BTC) &rarr; H
  + finds UTXO ≥ 9 BTC in G's wallet
  + sends the UTXO to H as input
  + H sends output of 1 BTC to G

+ can't tear UTXO apart into smaller value; like actual bill
+ can't gather & add up UTXO into larger UTXO

---

# Algorithms & Techniques: Public-Key Cryptography

`combination of hashing & encryption used for securing various elements of blockchain`

## Public-key cryptography

Symmetric key encryption uses same key for encryption and decryption

+ easy to derive secret key from the encrypted data
+ key distribution; how do you pass the key to the participant transacting?

**Asymmetric key encryption**

participants A and B

+ authenticate sender & receiver
  + A send transaction data encrypted by A<sub>private</sub>, then encrypted by B<sub>public</sub>
  + B will decrypt data with B<sub>private</sub>, then use A<sub>public</sub>to decrypt data

`Rivest Shamir Adleman (RSA) algorithm`

: passwordless user authentication

+ block chains need more efficient & stronger algorithm

`Elliptic Curve Cryptography (ECC) algorithm`

: used in bitcoin & Ethereum block chain

+ stronger than RSA  for given number of bits
  + ECC 256 bits of key pair is equal in strength to about RSA 3072 bits of key pair

## Hashing

: hash function/hashing transforms & maps an arbitrary length of input data value to a unique fixed length value, used for generating account addresses, digital signatures, transaction hash, state hash, receipt hash, and block header hash

Requirements of hash function

: achieved by choosing strong algorithm and using appropriately large number of bits in hash value

1. algorithm should be one-way function, collision free
2. hash value uniquely represents original items hashed

Common functions: SHA-3, SHA-256, Keccak
Common hash size: 256 bits

**Two approaches for hashing**

+ `Simple hashing`
  + all data items are linearly arranged & hashed
  + used when
    + fixed number of items to be hashed(block header)
    + verifying composite block integrity(not individal item integrity)

+ `Merkle tree hashing`
  + data is at the leaf noeds of the tree, leaves are pairwise hash to arrive at the same hash value as a simple hash
  + used when
    + number of items differ from block to block(transaction hash, state hash, receipt hash)
  + helps efficiency of repeated operations, like transaction modification & state changes from one block to the next

## Transaction integrity

1. secure a unique account address
2. authorization of the transaction by the sender through digital signing
3. verification of the content of the transaction is not modified

**Address of accounts**

1. 256-bit random number generated & designated as private key, kept secure & locked using passphrase
2. ECC algorithm applied to private key to get unique public key
3. hashing applied to public key to obtain account address

**Transaction initiated by the address**

1. transaction for transferring assets will have to be authorized(non-repudiable, unmodifiable)
2. examined digital signing process, then apply to transaction
3. data hashed &rarr; digital signature
4. encrypt the hash with private key of participant originating the transaction
5. receiver gets original data & digital signature to compare for verification of the integrity of document
+ time stamp, nonce, account balances, sufficiency of fees are also verified
6. if match, accept transaction; otherwise, reject

## Securing Blockchain

Main components of Ethereum block

: block header, transaction hash, transaction root, state hash, state root

**Integrity of a block**

1. block header contents not tampered with
2. transactions not tampered with
3. state transactions are computed, hashed and verified

+ Hashes of transaction in a block are processed in tree structure
  + to verify transaction, only one path to the tree has to be checked; rather than going through the entire set of transactions
  + to recompute state root hash for state change, only the affected path in Merkle tree needs to be recomputed

**Block hash in Ethereum**
+ block of all the elements in the block header, including transaction root and state root hashes
+ computed by applying a variant of SHA-3 algorithm called Keccak and al the items of the block header
+ computed by first computing the state root hash, transaction root hash and then receipt root hash
  + these roots and all the other items in header are hash together with variable nodes to solve proof of work puzzle
+ important purposes
  + verification of integrity of block & transactions
  + formation of the chain link by embedding previous block hash in current block header

# Decentralized Systems

+ securing the chain using specific protocols, validating the transaction/blocks for tamper proofing, verifying the availability of resources for transaction, executing/confirming the transactions

## Trust Train

1. Validate transaction
2. Verify gas and resources
   + about 20 criteria that have to be checked before a transaction
   + Ex. Ethereum: syntax, transaction signature, time stamp, nonce, gas limit, sender account balance, fuel, gas points, hash, ...
3. Select set of transactions to create a block
   + Merkle tree hash of validated transactions is computed
   + Ex. Ethereum: All miners execute transaction for transfer/smart contrants
     + state resulting from transaction execution are used in computing Merkle tree hash of the states(state root of block header)
     + receipt root of block header is computed
4. Execute transaction to get a new state
5. Form a new block
   + Proof of Work: consensus protocol used by Bitcoin Blockchain and Ethereum
   + Compute hash of block header elements that is a fixed value(BHash) & a nonce that is a variable &rarr; H(Hash)
     + If H < F(Difficulty) : puzzle solved, finalize & broadcast winning block to get verified by other miners
     + Else : repeat process after changing nonce value
6. Work towards consensus
7. Finalize the block by the bidder
8. New block added to chain and confirmed

# Robustness

: ability to satisfactorily manage exceptional situations

## When more than one miner solves the consensus puzzle

### Chain Split

+ chain splited into ommer blocks
+ then winner of next cycle for block creation consolidates to a single chain within a cycle
+ uncle block not included in main chain, only one block is accepted
+ new blocks are added to the main chain not to runner-up chains

## Double spending

: digital currency/consumables reused in transactions

+ Allow only the first transaction that reference the digital asset and reject the rest
+ Ex. Ethereum: combination of account number & global nonce is used to address the doublet spending issue
  + Everytime transaction is initiated by an account, global nonce is included in transaction
  + nonce incremented
  + timestamp of nonce in transaction should be unique & verfied to prevent any double use of digital asset

# Forks

: process in evolutionary path of the nascent technology enabling blockchain

+ like release of software pathces and new versions of operating systems repectively
+ mechanisms that add to the robustness of Blockchain framework
+ well-managed forks help build credibility in Blockchain by proving approaches to manage unexpected faults & planned improvements

## Hard Fork

: major change in protocol

## Soft Fork

: minor process adjustment carried out typically by bootstraping a new software to the already running processes, like software patch or bug fix