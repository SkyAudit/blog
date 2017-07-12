+++
draft = true
title = "Further Clarification of Obelisk"
tags = [
    "Decentralized",
    "Skycoin",
]
date = "2014-06-19"
categories = [
    "Ideology",
    "Information",
    "Obelisk",
]
description = "Skycoin has a blockchain. The blockchain is in skycoin/src/coin. That parses the blocks and deals with unspent outputs and transactions."
+++

Skycoin has a blockchain. The blockchain is in skycoin/src/coin. That parses the blocks and deals with unspent outputs and transactions.

Skywire is the daemon and has a "service architecture". It can run services, such as blockchain syncing service and other things. The meshnet is currently being implemented as a service on top of Skywire (although this may need to change).

The consensus mechanism is outside of the blockchain. Obelisk nodes (probably will be implemented as a Skywire service) have a blockchain. Each node has a public key. The public key identifies the Obelisk node. Each Obelisk node has its own blockchain (there are no coins in this chain). The node creates a new block and signs it with its private key. The Obelisk blockchains are used to negotiate consensus (determining the head block in the Skycoin blockchain). Obelisk uses Ben-Ors for randomized consensus. Each Obelisk node has a list of other nodes it subscribes to. Those nodes influence consensus and voting decisions for the local node. For non-pathological network topologies, the local consensus provably converges in to a global consensus.

Each node votes on the next block in the chain. A node purposes the next block and the nodes vote on the successor. The votes are published in the blocks in the Obelisk blockchain for each node. Your node votes randomly between the alternative and flips its vote every once in a while. Once 40% of your peers (the nodes you are subscribed to) have reached a consensus, you switch to that candidate. The network can vote on multiple forks at once, it does not slow down waiting for a consensus. The forks are pruned to a single chain over time. Splits of two or three block are normal, but after a few confirmations the probability of the block being reverted decreases exponentially to zero. If a transaction has been executed on all candidate  chains, then it essentially executed, even if the particular consensus chain has not been decided yet.

That is binary Ben-Ors and Skycoin will use something slightly more advanced, that is faster when there are multiple successor blocks to choose from in the consensus set. Randomization is important to keep sub-graphs of the network from getting stuck. The voting process is a form of "annealing" where each node will arrive at the global consensus independently, only from its local information.

The consensus process happens in public. A node publishes blocks, signs them with private key and the blocks are replicated peer-to-peer between subscribers of the chain. Then there are "consensus oracles" which are nodes that are used to verify consensus but do not influence consensus. So you might choose the public keys of a few exchanges and a few trusted community members and your node will use those to detect if something is wrong. This is used to detect netsplits. This also protects against an attack, where a hacker controls your router and can control the peers you are able to connect to.

If a node shows up to network and tries to get the network to accept a different chain (51% attack, reverting transactions), it usually gets ignored. Most 51% attacks require node behavior which is automaticly detected and results in subscriber node  removing the node from their trust list. The easiest 51% attack strategy is easy to detect and prove with mathematical certainty that it was intended as an attempt to revert transactions, because it require backdating block consensus decisions. It requires publishing two signed blocks with the same sequence number, so we just made this an automatically bannable offence for a node.

We are trying to eliminate the last possible 51% attack, which is when a subnetwork of nodes goes offline (netsplit attack) and then rejoins the network with a different blockchain consensus and tries to force this on the network to revert transactions. Most of these attacks will fail, because the subnetwork will not have enough influence and will accept the main network consensus.

This attack is still very difficult to pull off. One solution is when there is a successful 51% attack is to freeze the network and let each node/user individually which chain is the valid one and let people ban the attacking nodes by hand. The consensus oracle allow each node with high probability to know if the state is synced and if global consensus has been reached or if they are part of a netsplit subgraph. We think its possible for each node to know with high probability of correctness from local information, whether a node was offline during a consensus decision and then just ignore nodes that were offline who suddenly appear and try to force a chain fork on the network.

In Bitcoin, if you have the most hashing power, you can revert transactions whenever you want.

In Skycoin, to revert transactions
- you much control a large number of nodes
- the nodes you control must be "influential" and trusted within the network topology
- your nodes need to exhibit extremely blatant pathological attack behavior without the behavior being detected, because detection would result in losing the trust relationships you need to attack the network.
- your nodes need to be in a pathological attack topology, without it being detected (most bot nodes will be trusted by few humans and be very obvious)
- you must be able get the nodes you control to collude in a way that results in a successful attack (this is actually not very straightforward)
- if the attack succeeds, you must prevent the network from reverting the attack by hand (very difficult if people lost coins or money because of the attack)

To prove its 51% attack proof, you have to write down the assumptions you are making and then create a simple mathematical model and then prove the conditions under which things can and cannot happen in the model. Once you know the conditions that an attack is possible under, you try to eliminate them and if you cant eliminate them, you make them as difficult as possible. You increase the cost of an attack and you reduce the probability that a specific attack will succeed. Then you reduce the payoff and incentives for the attack.

The consensus process is simple and easy to model, but non-intuitive without seeing it. There will be a javascript site eventually that has an animated consensus process you can play with.
