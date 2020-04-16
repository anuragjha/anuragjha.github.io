---
layout: post
title:  "Difference between TradeLens, we.trade & Marco Polo?"
date:   2020-04-18 09:00:00 -0700
categories: blockchain
---

Comparing consensus, node design, business process, and participants. Which platform is the most promising and why?

TradeLens, we.trade & Marco Polo are all initiatives to use blockchain technology to digitize trade and interaction between different entities. Solutions from all the above companies work only for verified participants and therefore use private permissioned blockchain. TradeLens and we.trade use HyperLedger Fabric and Marco Polo uses r3 Corda as the underlying blockchain platform.

TradeLens is digitizing the supply chain and helping the shipping industry to record all information and movement of the product starting from source to destination. Thus eliminating all cross-checks and some coordination required in the supply chain process today making the process faster.
We.trade is working towards building what essentially is a reputation system. Where entities are referenced from their banks and registered into the network. Once an entity becomes a participant of the network then other participants can trust the reputation and choose to do trade with them. It allows for private transactions between trading participants.

Marco Polo is creating a network of Trade and working capital finance. Verified entities can trade and request for funds on the network using one to one communication such that no other participant is privy to the contents of the transaction.

TradeLens utilizes HyperLedger Fabric. Hyperledger is modular by design; consensus, privacy, and membership services are plug and play. This allows Hyperledger instances to use different consensus protocols. If a blockchain instance is deployed only for a single entity then Crash Fault tolerance consensus protocol is more appropriate. If blockchain is being used by multiple parties, then the Byzantine fault tolerance consensus protocol is a must. HyperLedger uses Practical Byzantine Fault Tolerance (PBFT) to achieve consensus.

This means Hyperledger has permissioned voting-based consensus. In voting-based consensus, nodes send messages to other nodes in the network. As the number of nodes in the network increases, the time required to reach consensus increases. Therefore there is a trade-off between scalability and speed.

We.trade utilizes “Channel” a feature of Hyperledger Fabric. It allows having multiple networks within the main network. This enables data scoping, where some information on the network can be hidden from the rest of the network. Channel is a private subnet of communication between two or more members in the network. Each channel has one ledger. Only the participants of the channel have the copies of the ledger corresponding to the channel. A node maintains a ledger for each of the channels they are participating in.

There are 3 different types of nodes in HyperLedger Fabric. They are Client, Peer, and Orderer. The client connects to the blockchain through a peer and submits the transactions. Peer keeps and maintains a copy of the Ledger. In general, a Peer does not participate in enforcing the consensus. If a Peer wants to participate in the consensus process they have to become an Endorser. An Endorser will endorse a transaction by signing on it, thus voting for the validity of the transaction.

An Orderer is a communication channel between clients and peers, so simply put an Orderer transports the transaction in an orderly fashion from clients to the Endorsers. An Orderer implements a pub-sub model, it acts as a broker. Clients publish a transaction to a channel and all the peers subscribed to that channel will receive the transactions to that channel in an orderly fashion. The endorser will then start the process to validate the transaction and sign if it thinks so.

This motivates invested parties in the supply chain and in finance & trade to use private transactions while still leveraging immutability and security in a trustless network thus ensuring a secure neutral ground to build a history of transactional information.

Marco Polo, on the other hand, utilizes r3 Corda. Allows private transactions between legally identifiable counterparties in an easy to use way. The identity of the nodes or a participant is known and verifiable. Identities can be found from Network Map Service. A participant goes through the KYC process to obtain an identity certificate and when they have to join the network they publish the certificate to network map service. Every contract has to reference a Legal contract that is binding in the real world. So transactions in Corda can be used to resolve a legal matter.

Corda uses UTXO (unspent transaction output) model. HyperLedger also aims to switch from Account/Balance (like that of Ethereum) to the UTXO model (like that of Bitcoin). In Corda, when one entity transacts with the other, the message is only sent to the intended participant. This transaction communication model differs from the traditional method of avoiding double spending where every interested node knows the occurrence of any transaction. In the Corda Notary pool, which is a set of nodes in the network act as a witness to all transactions and avoids the double-spending problem.

Corda leverages Uniqueness consensus. This consensus enforces that all of the input required for the different transactions is unique. Before Uniqueness consensus, a transaction has to pass through a Validity consensus. Here a signer signs that the transaction is valid for the given state of the system. Set of nodes in the Notary pool operate in Byzantine fault tolerance (BFT) consensus, they only sign a transaction if it does not represent a double-spend attempt.

Notaries do not see the content of the transaction. They only see the hash of the transaction and the index of the previous level output that is going to be updated, which is enough to verify the validity of the transaction. Thus the content of the transaction remains hidden from the notaries. The hash here corresponds particularly to the root hash of the Merkle tree. This help in concluding that particular input is part of the tree through root hash and a Merkle proof. Merkle proof is the level-wise hash of the siblings of the nodes in the transaction chain. The transaction chain is the path that connects all nodes from root to the new proposed transaction. Index here refers to the input of the previous state that is being updated with the new transaction.

Corda allows off-ledger facts to be stored and to be a part of the ecosystem and are contained in peers known as Oracles. Corda also has features such as a time window, which can help to add time conditions while validating a transaction. In Corda, Flow Framework is used to allow participating entities to coordinate point to point communication without a central container. Implementers of the flow are allowed flexibility to define custom flows. Flows are a predefined sequence of steps to be taken to agree on an update to the ledger.

Like in HyperLedger, Corda global network can be arranged into logical groups of nodes or a sub-network for a common business purpose. This subnetwork is known as a Business network in Corda. Each Business network can be configured for membership criteria, privacy, governance, business logic, and asset types. Again like HyperLedger in Corda, one can plug and play Consensus and other network parameters to suit business needs. Notary pool nodes are responsible for enforcing the given consensus model.

Users of Marco polo want to enforce business agreements between trading partners that have legal binding. Different trade organizations can be on the same network and still function in a separate environment privately and securely.

Needless to say that all three companies have beautifully abstracted away the complexity of blockchain and still provide all the benefits of the decentralized immutable ledger for the business use-case. HyperLedger is backed by Linux Foundation and IBM but in my opinion, r3 Corda better represents the needs of the businesses. Marco Polo built on Corda is more mature as a product for businesses to adapt comfortably. One of the main reasons is that it is connected to real-world legal contracts. Has the UTXO model which more scalable as compared to Accounts/Balance model as it allows parallel transactions. Has the concept of Notary pools which abstract away consensus model being used so business owners can get on with business processes they intend to implement.
