- [Basics: Why Smart Contracts?](#basics-why-smart-contracts)
  - [Smart Contracts Defined](#smart-contracts-defined)
    - [Elements of smart contract](#elements-of-smart-contract)
    - [Three steps in development of smart contract](#three-steps-in-development-of-smart-contract)
  - [Processing Smart Contracts](#processing-smart-contracts)
    - [Address](#address)
    - [Compile artifacts](#compile-artifacts)
  - [Deploying Smart Contracts](#deploying-smart-contracts)
- [Solidity](#solidity)
  - [Structure](#structure)
    - [Inherit other smart contract](#inherit-other-smart-contract)
    - [Function definition](#function-definition)
  - [Basic Data Types & Statements](#basic-data-types--statements)
  - [Specific Data Types](#specific-data-types)
  - [Data Structures](#data-structures)
  - [Access Modifiers & Applications](#access-modifiers--applications)
    - [Modifiers](#modifiers)
- [Putting it all together](#putting-it-all-together)
  - [Time Elements](#time-elements)
    - [now](#now)
  - [Validation & Test](#validation--test)
    - [by using Modifiers outside Function](#by-using-modifiers-outside-function)
  - [Client Application](#client-application)
    - [Event](#event)
- [Best Practices](#best-practices)
  - [Evaluating & Designing Smart Contracts](#evaluating--designing-smart-contracts)
  - [Designing Smart Contracts](#designing-smart-contracts)
  - [Remix IDE](#remix-ide)

# Basics: Why Smart Contracts?

`Smart contract`

+ centerpiece and main thrust of Ethereum blockchain
+ improper design and coding of smart contract result in significant failures such as DAO hack and Parity wallet lockup
+ many variations of smart contracts are prevalent in blockchain context
  + Bitcoin has script feature includes rules & policies
    + simple(limited) conditional transfer of value through an embedded script
    + Linux Foundation's Hyperledger blockchain has smart contract feature 'Chaincde', written in Go, and executed in Docker(lightweight container technology for executing programs) environment
+ Ethereum is working smart contract layer supports any arbitrary code execution over blockchain
+ addresses need for an application specific validation for blockchain applications
+ facilitates transaction for transfer of assets other than value or cryptocurrency
+ allows specification of rules for operation on blockchain
+ facilitates implementation of policies for transfer of assets in decentralized netwokr
+ add programmability and intelligence to blockchain
+ represents business login layer with actual logic coded in special high level language
+ embeds function that can be invoked by messages like function calls
  + messages/input parameters for a message are specified in transaction
+ provieds a layer of computation logic that can be executed on blockchain, thus availing feature enabled by blockchain framework
+ Ethereum Stack
  + Verticals: End User Applications
  + Application Framework: Smart Contracts
  + Ethereum Blockchain and Ethereum Virtual Machine(EVM)
  + Peer-to-Peer Network and Operating Systems
  + Hardware

**Bitcoin transaction vs Smart contract transaction**

+ Bitcoin transaction
  + all transactions are about SendValue
+ Blockchain supports smart contract
  + transaction could embed function implemented by smart contract
  + Ex. Voting smart contract: ValidateVoter, Vote, Count, DeclareWinner

**Existing system vs Smart contract**

+ Smart contract
  + all operations are transaprent and are recorded on blockchain
  + customers can access tools without intermediary (like a bank)

**Problems smart contract can solve**

+ business transaction may involve rules, policies, laws, regulations, governing contexts
+ smart contract allows real world constraints to be realized on blockchain, enables decentralized application of arbitrary complexity to be implemented on blockchain
+ run spectrum from supply chains to disaster recovery
+ allows implementation of rules, policies, methods for governance and provenance

`Remix IDE` : web interface for deploying & running transactions in blockchain, supports compiler/runtime test environments/JavaScript VM, Injected Web3 like Metamask, Web3 Provider

## Smart Contracts Defined

+ smart contract stores variables in state variables, and we can retrieve how variables change over blocks

### Elements of smart contract

+ pragma directive
+ name of contract
+ data/state variable define state of contract
+ collection of function to carry out intent of smart contract
+ identifiers representing thesse elements are erstricted to ASCII character set
+ follow camel case convention

### Three steps in development of smart contract

1. Design
```
- uint storedData
- set (uint)
- get () : returns uint
- increment (uint n)
- decrement (uint n)
```
2. Code
```sol
pragma solidity ^0.4.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() constant public returns (uint) {
        return storedData;
    }

    function increment (uint n) public {
        storedData = storedData + n;
        return;
    }

    function decrement (uint n) public {
        storedData = storedData - n;
        return;
    }
}
```
3. Test

## Processing Smart Contracts

### Address

: computed by hashing account number of externally owned account UI and nonce

### Compile artifacts

+ Name of contract
+ Contract Bytecode for deploying: bytecode executed for instantiating smart contract on EVM
+ Application Binary Interface(ABI) for application that smart contract interact with deployed bytecode
+ Web3 Deploy module: provides script code for invoking smart contract form web application
  + json script to web application to invoke smart contract function
  + script for programmatically deploying smart contract from web application
+ Gas estimates: provides gas estimates for deploying smart contract and for the function invocation
+ Function hashes: first 4 bytes of function signatures to facilitate function invocation by transaction
+ Instance BytecoInstance Bytecode: bytecode of smart contract instance

## Deploying Smart Contracts

<img src="-/deploying.png" width=500>

1. contract address is generated by hasing  sender's account address and its nonce
  + target account 0 or null: reserved for smart contract creation and deployment, meaning creating new smart contract using its payload fees
    + payload of transaction contains bytecode for smart contract
    + this code is executed as part of transaction execution to instantiate bytecode for actual smart contract
2. execute smart contract creation transaction &rarr; results in deployment of smart contract code on EVM, permanently stored in EVM for future invocation
     + this transaction goes through all regular verification/validation specified in Ethereum blockchain protocol
3. block creation and transaction confirmation by full nodes(a program that fully validates transactions and blocks) &rarr; deploys same contract on all nodes, providing consistent execution when regular transaction with function messages are invoked on smart contract

+ other approaches for deploying smart contract
  + deployed from Remix IDE, another smart contract, CLI, another high-level language application, web applicaton, ...


# Solidity

+ high level language for implementing smart contract and to target Ethereum Virtual Machine
+ combination of JavaScript, Java, C++

## Structure

1. Data or state variables
2. Functions:
   1. Constructor
   2. Fallback
   3. View
   4. Pure
   5. Public
   6. Private
   7. Internal
   8. External
3. User defined types in struct and enums
4. Modifiers

### Inherit other smart contract

Ex. StandardPolicies inherited by NYPolicies

```sol
contract StandardPolicies{...}

contract NYPolicies is StandardPolicies {
  // plus other policies...
}
```

### Function definition

```sol
function header { function code }
```

+ function header: can be simple as anonymous noname function to complex function header loaded with details
  + function nameOfFunction (parameters) visibilityModifier accessModifiers returns (returnParameters)
+ function code: contains local data & statements to process data and returns results of processing

## Basic Data Types & Statements

+ uint: unsigned int of 256 bits
+ int: integer positive and negative value accepted 256 bits
+ string: string of characters
+ bool: supports logic true and false value
+ default modifier is private, public modifier is explicitly stated
+ public data has default accessor/getter function
+ assignment statement, if-else, why, for, ... available

**BidderData.sol**

```sol
pragma solidity ^0.4.0;

contract Bidder {
    string public name; 
    uint public bidAmount = 200000;
    bool public eligible;
    uint constant minBid = 1000;
}
```

## Specific Data Types

+ Address
  + special Solidity define composite data type
  + holds 20-byte Ethereum address
  + Recall address: reference address to access smart contract
  + `<address>.balance (uint256)` contains balance of account in Wei
  + `<address>.transfer(uint256 amount)` transfer given amount of Wei to Address
+ Mapping
  + versatile data structure similar to key(secure hash of simple Solidity data type like address and key-value pair can be any arbitrary type) value store or hash table
  + ```
    mapping(uint => string) phoneToName;
    ```
  + ```
    struct customer {
      uint idNum;
      string name;
      uint bidAmount;
    }

    mapping(address => customer) custData;
    ```
+ Message
  + complex data type specific to smart contract
  + represents call that can be used to invoke function of smart contract
  + msg.sender: holds address of sender
  + msg.value: has value in Wei sent by sender
  + ```
    address adr = msg.sender
    uint amt = msg.value
    ```

## Data Structures

+ Struct
  + composite data type of group of related data that can be referenced by single collective name
  + individual elements of struct can be accessed using dot notation
+ Array
+ Enum
  + allows for user defined data types with limited set of meaningful values
  + used for internal use and are not supported currently at the ABI level of Solidity
+ Time units
  + Unix Epoch time
    + used in timestamping block time when block is added to blockchain
    + all transactions confirmed by block also have block time as their `confirmation time`
    + variable called "now" defined by Solidity returns block timestamp; variable is often used for evaluating time-related conditions

```sol
pragma solidity ^0.4.0;

contract StateTransV2 {
    enum Stage {Init, Reg, Vote, Done} // coded as 0, 1, 2, 3
    Stage public stage;
    uint startTime;
    uint public timeNow;

    function StateTransV2() public {
        stage = Stage.Init;
        startTime = now;
    }

    function advanceState() public {
        timeNow = now;
        if(timeNow > (startTime+ 10 seconds)) {
            startTime = timeNow;
            if(stage == Stage.Init) {stage = Stage.Reg; return;}
            if(stage == Stage.Reg) {stage = Stage.Vote; return;}
            if(stage == Stage.Done) {stage = Stage.Done; return;}
            return;
        }
    }
}
```

## Access Modifiers & Applications

+ often require who/what/when function should be executed

### Modifiers

+ can change behavior of function
+ checks condition to call function (no recording on blockchain)

Ex. Rule : at least 5 sellers have registered

```sol
modifier atLeast5Sellers {
  require(numSellers >= 5);
  _; // function body is inserted where this symbol appears in definition of modifier
}

function buy (..) payable atLeast5Sellers .. returns(..)

// Function code: collect value and transfer digital item

// Assertion (itemTransferred);
```

# Putting it all together

## Time Elements

### now

```sol
startTime = now;
```

## Validation & Test

### by using Modifiers outside Function

```sol
modifier validStage(Stage reqStage) {
  require(stage == reqStage);
  _;
}

function vote (uint8 toProposal) public validStage(Stage.Vote) {
  Voter storage sender = voters[msg.sender];
  if(sender.voted || toProposal >= proposals.length) return;
  sender.voted = true;
  sender.vote = toProposal;
  proposals[toProposal].voteCount += sender.weight;
  if(now > (startTime + 20 seconds)) {
    stage = Stage.Done;
    votingCompleted();
  }
}

function winningProposal() public validStage(Stage.Done) constant returns (uint8 _winningProposal) {
  uint256 winningVoteCount = 0;
  for(uint8 prop = 0; prop < proposals.length; prop++) {
    if(proposals[prop].voteCount > winningVoteCount) {
      winningVoteCount = proposals[prop].voteCount;
      _winningProposal = prop;
    }
  }
}
```

## Client Application

### Event

```sol
// definition
event nameOfEvent(parameters);
```

+ track transaction
+ receive results
+ initiate pull request to receive info from smart contract

# Best Practices

## Evaluating & Designing Smart Contracts

**Blockchain suitable for**

+ Decentralized problems
+ Peer-to-peer transactions
+ Beyond boundaries of trust among unknown peers
+ Reqire validation, verification, recording of timestamped, immutable ledger
+ Autonomous operations guided by rules/policies

**Remember**

+ Smart contracts will be visible for all participants and will be executed on all full nodes
+ Solve single problem
+ Keep smart contract code simple
+ Blockchain is not data repository; keep only necessary data

## Designing Smart Contracts

**Remember**

+ Use appropriate data types
  + integer id for proposal than string
  + integer arithmetic for most of computational needs
+ Understand public visibility modifier for data
+ Maintain standard order for idfferent function types within smart contract, according to their visibility as specified in Solidity docs
  + constructor &rarr; fallback function &rarr; external &rarr; public &rarr; internal &rarr; private
+ Functions can have different modifiers
  + visibility modifiers &rarr; custom access modifiers
+ Pay attention to order of statements within function
+ Use modifiers declaration for implementing rules
  + for implementing rules, policies, regulations
+ Use events for notification
+ Beware of 'now'
  + used for approximate elapesd time comparison
  + Use secure hashing for protecting data
    + to protect visibility
    + keccak256(...) returns(bytes32)
      + computes Keccak-256 hash of params
    + sha256(...) returns(bytes32)
      + computes SHA-256 hash of params
    + ripemd160(...) returns(bytes20)
      + computes RIPEMD-160 hash of params
      + used for address calculation

## Remix IDE

**Remember**

+ Pay attention to Remix static analysis & console detail
+ Review compile details
  + Compile > Detail shows all artifacts generated
+ Remix transaction log for debugging
  + shows json version of all executed transaction