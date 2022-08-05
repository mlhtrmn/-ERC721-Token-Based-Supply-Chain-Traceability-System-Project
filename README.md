# ERC721-Token-Based-Supply-Chain-Traceability-System-Project

Abstract
Supply Chain Traceability is one of the most promising use cases to benefit from blockchain characteristics such as decentralization, immutability and transparency, without requiring to build prior trust relationships among entities. A plethora of blockchain-based supply chain traceability solutions has been proposed recently. However, current systems are limited to tracing simple goods that have not been part of a manufacturing process. I propose a system that allows for the traceability of manufactured goods, including their components. Products are represented using digital non-fungible tokens that are created on a blockchain for each batch of products that has been manufactured. To create a link between a product and the components that are needed to manufacture it, I propose ”token recipes” that define the amount of tokenized goods that are required for minting a new token. As input tokens are automatically and transparently consumed when creating a product token, the physical production process of composing a new good out of existing components is projected onto the ledger. This ultimately leads to the complete traceability of goods, including the origin of inputs.
Concept
I propose a blockchain-based, decentralized supply chain management system based on smart contracts. In order to provide comprehensive provenance information to consumers and producers, I maintain the relationship between resources and products in manufacturing processes. The concept is based on two key ideas: representing physical goods in the form of digital tokens, and recipes that enable their transformation. Additional functionality like certifying goods, transferring, splitting and combining tokens facilitates cross-business traceability.
Installation and Instructions
### TECH STACK
- Solidity ^0.0.7
- Truffle v5.1.65 - Node v15.4.0
### DEPENDENCIES
- truffle [npm](https://www.npmjs.com/package/truffle) - installation:
```bash
npm i -g truffle
  ```
- @openzeppelin/contracts [npm](https://www.npmjs.com/package/openzeppelin-solidity) - installation:
```bash
npm i @openzeppelin/contracts@solc-0.7 -s ```
### RUN
   - Go to project root folder:
- Install dependencies via npm:
```bash npm i ```
- Start the truffle development server (which is a test-rpc a.k.a ganache-cli anyways)
 ```bash
truffle develop ```
 - And start playing around with the contracts after migration, how to interact with your contracts is [here](https://www.trufflesuite.com/docs/truffle/getting-started/interacting-with-your-contracts)
 
 - Alternatively one can start the truffle console (for which you have to configure a development server in ./truffle- config.js)
```bash
truffle console
```
- More details for quick start on [truffle](https://www.trufflesuite.com/)
  Implementation
To achieve advanced traceability in supply chains, I propose a set of solidity contracts which are deployable on any EVM compatible blockchain. To achieve widespread compatibility, the ERC 721 interface is implemented. As a result, standard Ethereum wallets supporting the interface can be used for managing and trading tokens. Since minting of tokens is not provided by default, such actions have to be taken by using a tailored user interface. To extend the feature set, I inherit from the well tested token contract implementation that is supplied by OpenZeppelin11, a library for secure smart contract development. In our modified implementation we suggest the representation of product batches as unique data structures which hold certain characteristics, such as the goods which have been consumed during the production process. Each batch of products corresponds to one token that holds unique features. Resource suppliers and manufacturers hold their own set of token contracts, one for each kind of product. To facilitate the deployment of novel token contracts, they can utilize a factory contract. While a token contract’s ownership always remains with the initial deployer, the batches may be traded and change owners.
Entities
- On truffle, develop a console. We have these three entities:
1. Owner (contract deployer)
2. Consumer1 (simulating a consumer/intermediary)
3. Consumer2 (simulating another consumer/intermediary)
 
StateVerification Contract Interactions
- StateVerification contract is deployed and interacted as ‘stateAuthority’ object
- Created entities are approved by ‘stateAuthority’ using
‘insertVerifiedEntity(address_addr)’ function as follows:
1. This method has isOwner() modifier, allowing only the contract owner to add
entities to the whitelist. - Owner is whitelisted:
 - Consumer1 is whitelisted:
 - Consumer2 is whitelisted:
 - Aforementioned entities are verified by ‘stateAuthority’ using ‘verify(address _addr)’ function as follows:

 - Another consumer/intermediary called Consumer3 is created as follows:
- A consumer could be removed from the whitelist using ‘removeVerifiedEntity(address_address)’ function as follows:
  ProductProvenance Contract Interactions
- ProductProvenance contract is deployed and can be interacted as ‘supplyChain’
- A whitelisted consumer (i.e Consumer1, Consumer2) is allowed to mint a token/product.
1. This operation is done using mintProduct() function.
2. This function uses the built-in _mint() function, and replicates its default
behaviour.
3. Miinted token is transferred to message sender from the contract (Owner)
- Consumer1 minted a token:

 - trace the token history post-transfer, notice that transfer is occured:
- Consumer2 can transfer the token of Consumer1 to himself, but first Consumer1 should approve Consumer2 for a specific token to be transferred:
 
 - trace the token history post-transfer-approval, notice that the transfer is approved:
- Consumer2 could now complete the transfer, since it is approved by Consumer1:
 
 - trace the token history post-transfer-approval, notice that the transfer is occured:
- make a transfer to a non-whitelisted entity:
The receiver entity of this function is NOT whitelisted by the state authority.
 
 - mint a token as a non-whitelisted entity:
The receiver entity of this function is NOT whitelisted by the state authority.
- whitelist an entity as a consumer/intermediary: This function is restricted to the contract owner.
- remove a whitelisted address from state registry as a white-listed consumer/intermediary: This function is restricted to the contract owner.
- trace a token/product history as a non white-listed entity:
The caller entity of this function is NOT whitelisted by the state authority.
Evaluation
5.1. Gas costs
As contract code needs to be stored for every contract instance created on the blockchain, the code size should be decreased to minimize gas costs. With base costs of 88, 202gas for adding a new token batch in the classic approach and 92, 634gas when using a library and factors of 19, 698 and 27, 427gas for each input respectively, we set up the following equation,
x = 415, 854gas / (4, 432gas + 7, 729gas ∗ |S | )
where x is the equilibrium between using a library contract and classic deployment and S is the set of inputs. As a result, when having no inputs, the library is only cheaper in terms of gas for up to 93 minting calls while for ten inputs its only cheaper for up to 5 calls. In addition, token owners transfer split, merge and consume batches which takes more gas using a library due to the involved overhead in the proxy contract.
    
Conclusion
I have proposed a blockchain-based supply chain management system that enables tracing and tracking goods including their transformation in the production process using smart contracts. The system provides comprehensive provenance information by projecting product compositions onto the blockchain in the form of tokens. Hereby, shortcomings of current traceability systems regarding isolated data storage and lacking transformation information are tackled. Defining compositions for creating products and enforcing them using smart contract enables the documentation of consumed resources in the production process. Through this mechanism, products are not only traceable from production to retail but starting from resource exploitation. As a result, transparency is generated along the supply chain, providing comprehensible production information.
