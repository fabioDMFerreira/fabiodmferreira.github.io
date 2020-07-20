---
title: Mastering bitcoin
image: /assets/img/blog/ants-silhouette.jpg
description: >
  Notes from book "Mastering Bitcoin", by Andreas Antonopoulos.
---


- Database
  - Linked hash list
  - Block
    - Transactions (Advanced transactions)
  - Merkle tree
- Mining
  - Rules to add new blocks to Bitcoin
  - Fees and compensation
  - Mempool, orphan pool
- Networking
  - P2P network
  - Different node types
    - Wallets
      - Bloom Filter
- Cryptography
  - Keys and addresses

## Keys and addresses

- Base58CheckEncoded
  - Most of the data presented is BASE58CheckEncoded
  - First byte is the version prefix used to easily identify what the string is
      - *1* - bitcoin address
      - *3* - Pay-to-Script-hash address
      - *6p* - BIP-38 Encrypted Private Key
      - *5* - Private key with WIF
      - *K or L* - Private key with WIF compressed
      - *xpub* - BIP-32 Extended Public Key
      - *M or N* - Bitcoin Testnet Address
  - is a subset of Base64, using upper- and lowercase letters and numbers, but omitting some characters that are frequently mistaken for one another and can appear identical when displayed in certain fonts.
  - used to encode keys and addresses
- Private key
  - 256 bits shown as a 64 hexdecimal digits, each 4 bits
    - Example with WIF format: 5J3mBbAH58CpQ3Y5RNJpUKPE62SQ5tfcvU2JpbnkeyhfsYB1Jcn
  - Representations
    - Raw (32 bytes)
    - Hex (64 hexadecimal digits)
    - WIF (wallet import format) - Base58 of 64 hexadecimals plus version prefix (128) and checksum (32 bits)
    - WIF compressed - WIF with suffix 0x01 before encoding
      - is one byte longer than "uncompressed" version
      - The term “compressed private key” really means “private key from which only compressed public keys should be derived,
  - Encryption
    - BIP-38
      - encrypt private key with passphrase using AES and encode with Base58Check
      - Private key encrypted must be encoded with WIF format
      - Make key portable and secure to be stored in a media device
      - Starts with 6P
- Public key
  - calculated from private key
    - K (public key) = k (private key) * G (constant called generator point)
    - Elliptic curve multiplication (cryptographic function)
    - secp256k1 (a set of mathematical constants)
- Digital signatures
  - proof messages user authenticity.
  - generated with private key
  - validated with public key
- Bitcoin address
  - Example: 1J7mdg5rbQyUHENYdx39WVWK7fsLpEoXZy
  - hashed from public key
    - Key hashed = RIPEMD160(SHA256(K))
    - Payload = version prefix + Key hashed + checksum
    - Bitcoin address = Base58Encode(Payload)
  - string starts with digit "1"
  - normally encoded to Base58Check
  - transactions where normal bitcoin address is used are called pay-to-public-key-hash (P2PKH)
- Pay-to-Script Hash (P2SH) and Multisig addresses
  - string starts with digit "3"
  - designate the beneficiary of a bitcoin transaction as the hash of a script, instead of the owner of a public key
  - Introduced with standard BIP-16
- Vanity Address
  - customised address
    - Example: 1Kids11111111111111111111111111111
  - address with a vanity pattern
  - a vanity address is a personal choice and is an address like others

## Wallets

- keychain
- piece of software installed in a machine that acts as user interface
- Functionalities
  - store private and public keys
  - create and sign transactions
  - track owner transactions
  - track owner balance
  - generate addresses used to receive new coins
- types
  - Non deterministic wallet
    - each key is generated from a random number
    - JBOK (just a bunch of keys)
    - keys are not related
    - cumbersome to manage, backup and import
    - being replaced by deterministic wallets
    - known in *Bitcoin Core* as a type-0 wallet
  - Determinist wallet
    - all keys are generated from a master key, known as the seed
      - keys are related and can be used to generate each other if one has the original seed
    - use mnemonic to recover keys
    - key derivation methods
      - Most commonly used is the hierarchichal deterministic wallet (HD wallet)
        - use a tree structure
        - BIP-32
        - Advantages
          - organization
            - possible to define different functionalities for different branches of keys
          - security/privacy
            - allow create public keys without the need of a private key
            - possible to have a server generating keys without knowing the private key
            - Use case is a server that generates public keys for an online store
        - BIP-39 Mnemonic code words
          - 24-word mnemonic
          - Different from "brainwallets", which ones allow users to choose their words
          - algorithm
            1. 128 randoms bits + checksum => map each 11 bits to a 2048 dictionary word => mnemoic (sequence of words)
            2. PBKDF2(mnemonic + optional passphrase + salt ) => 512-bit seed (master key)
              - PBKDF2
                - stretch key function
                - use algorithm HMAC-SHA512
        - Seed
          - first 256 bits are the private key (m)
          - second 256 bits are the master chain code (c)
            - used to introduce entropy in fuction that generates child keys from parent key
          - Child key derivation (CKD)
            - derived using hash function on:
              - private or public key
              - chain code
              - index number
                - [0, 2( 31)-1] for normal derivation
                - [2( 31), (2 32)-1] for hardned derivation
            - child keys are derived from its parent chain code
            - used as bitcoin addresses and to sign transactions
              - they have the same features of any private key and public key
            - they are extend extend keys
              - each one has
                - 256 bits for the private key
                - 256 bits for the chain code
            - 2 types
              - extended private key
                - encoded with Base58Check - prefix version number "xprv"
              - extended public key
                - this key can generate a branch of public keys without private key help
                - encoded with Base58Check - prefix version number "xpub"
                - security issue
                  - extended public key with a child private key allows to generate a branch of private keys
                  - measure to solve this problem was "hardened child key derivation"
                    - public keys uses private keys chain code to generate other public keys
                    - Hardened derivation of a child key omits the parent public key
- best practices
  - Mnemonic code words, based on BIP-39
  - HD wallets, based on BIP-32
  - Multipurpose HD wallet structure, BIP-43
  - Multicurrency and multi account  wallets, BIP-44


## Transactions

- Transaction structure example
```
{
  "version": 1,
  "locktime": 0,
  "vin": [
    {
      "txid": "7957a35fe64f80d234d76d83a2a8f1a0d8149a41d81de548f0a65a8a999f6f18",
      "vout": 0,
      "scriptSig" : "3045022100884d142d86652a3f47ba4746ec719bbfbd040a570b1deccbb6498c75c4ae24cb02204b9f039ff08df09cbe9f6addac960298cad530a863ea8f53982c09db8f6e3813[ALL] 0484ecc0d46f1918b30928fa0e4ed99f16a0fb4fde0735e7ade8416ab9fe423cc5412336376789d172787ec3457eee41c04f4938de5cc17b4a10fa336a8d752adf",
      "sequence": 4294967295
    }
  ],
  "vout": [
    {
      "value": 0.01500000,
      "scriptPubKey": "OP_DUP OP_HASH160 ab68025513c3dbd2f7b92a94e0581f5d50f654e7 OP_EQUALVERIFY OP_CHECKSIG"
    },
    {
      "value": 0.08450000,
      "scriptPubKey": "OP_DUP OP_HASH160 7f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a8 OP_EQUALVERIFY OP_CHECKSIG",
    }
  ]
}
```
- Outputs (vin)
  - *Unspent transaction outputs* (UTXO)
    - List of outputs that are still spendable
    - Input is the possible output of other transaction
    - Coinbase transaction is the only one that does not need an input
        - is the first transaction in a block
        - is the reward for the miner
        - has a special type of input called coinbase
    - UTXO is used by wallets to calculate balance
  - components
    - amount in satoshis
      - bitcoin has 8 decimal places, the smaller unit is called satoshi
    - cryptographic puzzle that determines the conditions to be spend
      - known as lock script, witness script and scriptPubKey
- Inputs (vout)
  - components
    - reference to the transactions hash
    - output index (vout), identifying which output is referenced
    - sequence number where the UTXO is recorded in the blockchain
    - Unlocking script
      - most often is a digital signature and a public key proving the ownership of the bitcoin
- Fee
  - compensation to miners for securing blockchain
  - calculated based on transactions size (not transactions value)
  - SUM(inputs) - SUM(outputs)
    - Input value is in different transaction
- Transactions scenarios
  - If input value is larger than needed it is generated a change
    - Sender owns the change
  - Multiple inputs may be aggregated to pay a larger value.
  - Multiple inputs may be used to pay to different accounts
    - wasabi wallet uses a technique called coinjoin to create this kind of transactions
      - Improve transactions privacy
- Transaction language and script language
  - called *Script*
  - used to create conditions that allow UTXO to be spent
  - locking script (scriptPubKey on output) and unlocking script (scriptSig input) are written in Script language
  - transaction is validated by executing scriptSig and scriptPubKey combined
  - Turing incompletness
    - predictable execution times
    - no loops
  - Stateless verification
    - there is no state prior the execution
  - Stack language
- Digital signatures
  - uses algorithm ECDSA
  - Purposes
    - proves user owns private key
    - proof of authorization is undeniable
    - proves transaction can't be modified by anyone after it has been signed
  - Sig = Fsig(Fhash(m),dA)
    - m => transaction
    - dA => private key
    - Fhash => ECDSA
  - Verifying signature
    - Signature Hash types
      - A signature can commit to only a subset of the data in the transaction
      - SIGHASH flag
- Transactions types
  - Pay-to-Public-Key-Hash (P2PKH)
  - Multisignature
  - Pay-to-Script-Hash (P2SH)
    - script is replaced by fingerprint
    - to spend output user has to have script to generate the same fingerprint, with the addition of having the unlocking script
    - locking script is called redeem script
    - address encoded with Base58Check has prefix "3".
    - Benefits
      - created to reduce the size of complex scripts and this way avoid higher fees
      - shorter transactions
      - shift burden of constructing script to the recipient, instead of the sender
  - Data Recording Output (RETURN)
    - Used to save data in blockchain
    - Don't produce outputs to be spend
  - Timelock
    - Used to add a time condition to spend transaction output

## Network

- P2P network
  - all nodes are peers of each other
  - there are no special nodes
- Nodes functions
  - Blockchain data
  - Routing
    - maintain connection with peers
    - validate and propagate transactions and blocks
    - all nodes has routing function
  - Mining
  - Wallet services
- Nodes types and roles
  - Full nodes
    - have the 4 possible functions
    - can verify transactions without external services help
  - Simplified Payment Verification (SPV) node
    - maintain a subset of the blockchain and verify transactions
    - commonly has wallet services
    - downloads block headers (not including transactions)
    - verifies transactions by reference to their depth
    - check if transaction exist in block by requesting a merkle path proof
    - Bloom Filters
      - allow nodes to receive a subset of transactions
      - improves privacy
        - hide the transactions they are interested in
- Bitcoin Relay Network
  - network that attempts to minimize network latency
  - essencial for mining nodes
  - consist in special nodes hosted by Amazon distributed across the world hosted that communicate with nodes
- Network discovery
  - nodes connect via TCP, usually to port 8333
  - node sends information about himself to establish connection
    - nVersion
      - P2P protocol version
      - if compatible with other nodes a connection is established
    - nLocalServices
    - nTime
    - addrMe
    - addrYou
    - subver - sub-version showing the type of software running in the node
    - BestHeight - block height of the node
  - Nodes find each others by using DNS
  - nodes share addresses with which they are connected
  - nodes are continually connecting to each other as they lost connections
  - a node is limits the number of connections
  - after connection node will try to get a blockchain copy
- Transaction pools
- Orphan pools

## Blocks and blockchain
- Blockchain
  - linked list of blocks
  - each block has a reference to previous one
- top or tip
  - most recent block
- Block
  - container that aggregates transactions to be inserted in the blockchain
  - identified by
    - hash
      - digital fingerprint, made by hashing block header twice with SHA256 algorithm
      - doesn't exist in the block
    - height
      - number of blocks between first one and the last one
  - Structure
    - Block size (4 bytes)
    - Header (80 bytes)
      - version
      - hash to previous block
      - mining metadata
        - difficulty
        - timestamp
        - nonce
      - merkle tree root
    - Transactions counter (1-9 bytes)
    - Transactions (variable)
- Merkle tree
  - https://www.codementor.io/blog/merkle-trees-5h9arzd3n8
  - binary hash tree
  - efficient way to **verify** whether transaction belongs to block
    - needs to produce log2(N) 32 byte hash, called authentication path or merkle path
    - knowing the merkle root and the transactions of a block allows to verify if the transaction is in the block
      - checking the transaction exist in transactions list isn't enough because nodes may provide data that is intentionally or accidentally corrupted
  - digital fingerprint of entire transactions set
  - hash transactions pairs until find root
    - Use double-SHA256 algorithm to hash
    - Hashing 4 transactions consist in
      - Hash(Hash(A+B)+Hash(C+D)) = root merkle tree

## Mining and Consensus
The maximum amount of newly created bitcoin a miner can add to a block decreases approximately every four years

- Process by which transactions are validated
- Steps
  1. independent verification of each transaction (they should follow a criteria list)
  2. aggregate verified transactions into new blocks, coupled with computation proof (hash)
    - after verification the transactions join the memory pool where they wait to be joined into a block
  3. independent verification of each block (also follow a criteria list)
  4. selection of blocks with the most cumulative computation proof
    - nodes maintain 3 sets of nodes
      - those connected to the main change
      - those that form branches off the main chain (secondary chains)
        - Main chain may have branches of nodes that are sibling
          - they are kept as future reference, in case one of the branch extends and become the main chain
      - blocks that don't have parent, called orphans
- Compensation for mining
  - New coins created
  - Fees from all transaction
- Mining is done by nodes specialized in mining
- Forks
  - occur as temporary inconsistencies, which are resolved sooner or later as more blocks are added
- Changing Consensus Rules (forced forks)
  - Hard Fork
    - network diverge into two chains
    - two chains evolve independently operating under different consensus rules
    - nodes that don't upgrade to the new consensus rules can't participate in the new consensus mechanism
  - Soft Fork
    - forward-compatible change to the consensus rules
    - unupgraded clients to continue to operate in consensus with the new rules.
