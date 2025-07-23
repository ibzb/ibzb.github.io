# Bitcoin Developer Reference  

## Introduction  
Bitcoin, a decentralized digital cryptocurrency, was conceptualized by pseudonymous developer Satoshi Nakamoto. The foundational 2008 paper introduced Bitcoin's architecture, laying the groundwork for its implementation. This document formalizes the core Bitcoin protocol, focusing on its cryptographic constructs and architectural design.  

## Preliminaries  
Understanding Bitcoin requires familiarity with cryptographic primitives like **Proof of Work (PoW)** and **Merkle Trees**. These elements underpin Bitcoin's security and efficiency.  

### Proof of Work  
PoW ensures computational effort is invested to validate transactions. Bitcoin's PoW, based on Adam Back’s Hashcash, requires miners to find a nonce `n` such that:  
```  
H = SHA256²(msg || n) ≤ T  
```  
Here, `T` is the target difficulty. Verification is efficient, but solving the puzzle probabilistically demands significant effort.  

**FAQ**:  
**Q: What is Proof of Work's role in Bitcoin?**  
**A**: PoW secures the network by requiring miners to solve computational puzzles, ensuring transaction validity and preventing double-spending.  

### Merkle Trees  
Merkle Trees enable efficient data integrity verification. Leaves are hashes of data blocks, while parent nodes combine child hashes. This structure allows logarithmic scaling for updates.  

**Example**:  
If a data block changes, only its branch needs re-computation. Bitcoin duplicates the last node if an odd number of nodes exists, ensuring a complete binary tree.  

👉 [Learn more about blockchain security](https://bit.ly/okx-bonus)  

## Architecture  
Bitcoin's architecture revolves around the **blockchain**, a public ledger storing transactions. Blocks are mined by solving PoW puzzles, with each block referencing the previous via `HashPrevBlock`.  

### Blocks  
Blocks consist of a header and payload. The header includes:  
- `nVersion`: Block format version (currently 2).  
- `HashPrevBlock`: Double-SHA256 hash of the previous block.  
- `HashMerkleRoot`: Root of the Merkle Tree for transactions.  
- `nTime`: UNIX timestamp of block creation.  
- `nBits`: Target difficulty in compact format.  
- `nNonce`: Arbitrary value for PoW.  

**Table 1: Block Structure**  
| Field Name       | Type          | Description                          |  
|------------------|---------------|--------------------------------------|  
| nVersion         | int (4 bytes) | Block version                        |  
| HashPrevBlock    | uint256       | Hash of previous block               |  
| HashMerkleRoot   | uint256       | Merkle root                          |  
| nTime            | uint (4 bytes)| Timestamp                            |  
| nBits            | uint (4 bytes)| Target difficulty                    |  
| nNonce           | uint (4 bytes)| PoW nonce                            |  
| #vtx             | VarInt        | Number of transactions               |  
| vtx[]            | Transaction   | List of transactions                 |  

### Transactions  
Transactions transfer Bitcoin between users. Two types exist: **regular transactions** and **coinbase transactions** (rewarding miners).  

#### Regular Transactions  
Fields include:  
- `nVersion`: Transaction version (1).  
- `vin`: Inputs referencing prior outputs.  
- `vout`: Outputs specifying amounts and conditions.  
- `nLockTime`: Time until which transaction can be replaced.  

**Table 2: Regular Transaction Structure**  
| Field Name       | Type          | Description                          |  
|------------------|---------------|--------------------------------------|  
| nVersion         | int (4 bytes) | Transaction version                  |  
| #vin             | VarInt        | Number of inputs                     |  
| vin[]            | Input         | Transaction inputs                   |  
| #vout            | VarInt        | Number of outputs                    |  
| vout[]           | Output        | Transaction outputs                  |  
| nLockTime        | uint (4 bytes)| Lock time                            |  

#### Coinbase Transactions  
Unique in including the **coinbase field**, which encodes the block height and arbitrary data. Miners claim rewards via these transactions.  

**Table 3: Coinbase Transaction Structure**  
| Field Name       | Type          | Description                          |  
|------------------|---------------|--------------------------------------|  
| coinbaseLen      | VarInt        | Length of coinbase field             |  
| coinbase         | CScript       | Block height and arbitrary data      |  

## Bitcoin Ownership  
Ownership is enforced through **Script**, a stack-based language defining spending conditions.  

### Script  
Script combines challenge (`scriptPubkey`) and response (`scriptSig`) scripts. Validity is confirmed if the final stack evaluates to `true`.  

### Standard Transaction Types  
1. **Pay-to-Pubkey (P2PK)**: Directly sends Bitcoin to a public key.  
2. **Pay-to-PubkeyHash (P2PKH)**: Uses a hashed public key (address).  
3. **Pay-to-ScriptHash (P2SH)**: Allows complex conditions via a script hash.  
4. **Multisig**: Requires multiple signatures.  
5. **Nulldata**: Embeds arbitrary data in transactions.  

**FAQ**:  
**Q: What is a P2SH transaction?**  
**A**: P2SH allows sending Bitcoin to a script hash, enabling advanced features like multisignature wallets.  

## Blockchain Structure  
The **blockchain** is a distributed ledger preventing double-spending. Each block’s `HashPrevBlock` ensures chronological order.  

### Mining  
Miners compete to solve PoW puzzles. Steps include:  
1. Collecting transactions and validating them.  
2. Building a block with a valid Merkle root.  
3. Adjusting `nNonce` and `coinbase` to find a hash below `T`.  

**FAQ**:  
**Q: How does Bitcoin mining work?**  
**A**: Miners validate transactions by solving cryptographic puzzles, securing the network and earning rewards.  

👉 [Explore cryptocurrency mining](https://bit.ly/okx-bonus)  

### Proof of Work in Mining  
The PoW solution must satisfy:  
```  
SHA256²(nVersion || HashPrevBlock || HashMerkleRoot || nTime || nBits || nNonce) ≤ T  
```  
Miners adjust `nNonce` and `coinbase` to find valid solutions.  

## Appendices  
### Data Types  
| Type       | Size (bytes) | Description                     |  
|------------|--------------|---------------------------------|  
| int        | 4            | Signed integer (little-endian) |  
| uint256    | 32           | SHA256 hash                     |  
| VarInt     | 1–9          | Variable-length integer         |  

### Formulas  
- **Transaction Fees**:  
  ```  
  TxFee = TxFeeRate × ⌈TxSize / 1000⌉  
  ```  
- **Dust Transactions**:  
  ```  
  MinTxFeeRate × (TxOutSize + 148) / nValue > 1/3  
  ```  

## Conclusion  
This reference outlines Bitcoin’s core protocol, emphasizing cryptographic foundations and transaction mechanics. By integrating SEO-friendly keywords like **blockchain**, **cryptocurrency**, and **mining**, this document balances technical depth with accessibility.  

**Final FAQ**:  
**Q: Why is Bitcoin secure?**  
**A**: Bitcoin’s security stems from PoW, cryptographic hashing, and decentralized consensus, making tampering infeasible.