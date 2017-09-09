### It's Time to SAFE the Internet.

Introducing SAFE, the decentralised privacy-first storage and communication network.

Date: Sun 5th Feb
Time: 12:00
Speaker: Benjamin Kampmann (@gnuicornBen)

Root problem of security is the central server paradigm.

Need to move to more decentralised systems (e.g. P2P) for resilience and safety.

Big problem is we always address things by **location**. URLs! Means we're reliant on DNS services -- we still need to go the library to get a book!

We should be using content address, not location. This is URIs.

Content addressing is used in P2P BitTorrent. You can use checksums for this!

However, having each client storing checksums of everything is hard to keep up to date! So BitTorrent uses **Kademlia Distrubted Hash Tables**. It defines distance between nodes and distance between content and a node.

Very useful -- it obfuscates the source/destination and is resilient to central failures.

If so, why isn't every system online using decentralised

Because **consensus** if difficult. How to do consensus without a single central master?

#### Consensus

Unaninimous decisions (like Paxos, Raft) in which everyone grees on a new state.

However, doing this for every transaction would just take too long and wouldn't scale.

What about bitcoin? Majority voting on the global state (blockchain) following a **predefined process** (longest chain) on a reduced set of possible options (crypto puzzles).

Why not use bitcoin? Getting the majority **takes time**. e.g. blockchain has a 10 min time window, not acceptable for general web.

#### SAFE

Safe Access for Everyone

P2P decentralised storage network. With BitTorrent-like routing, but multiple different types of routing personas (much like how Raft/Paxos have different machine roles).

XOR routing is used, like BitTorrent(?)

Built-in data retention deue to data centralised network. Max number of replicates are 8.
es)
Encrypted by default, before it leaves your computer. Even your public data is encrypted.

SAFE has many data types:

* Authentication
* Immudatable data
* Mutable data*
* Currency
    - 'money forgets' is an important concept in our legal system
    - this allows currency to do this

How it works:

* run safe launcher, entry point/control centre to the system
* bind to specific names (hash values)

Use SAFE API!

Contribute using Rust -- go to https://github.com/maidsafe
