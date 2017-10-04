+++
draft = true
title = "Clarification on the Personal Blockchain"
tags = [
    "Decentralization",
]
date = "2014-05-06"
categories = [
    "Ideology",
    "Information",
    "Personal Blockchain",
]
+++
## Comment:
Quote from: **FrictionlessCoin** on May 06, 2014, 01:08:09 PM
>I still however am have trouble understanding how the protocol works and this
notion of 'personal blockchains'.  Can you point me to the documentation that
describes this better?

For a person blockchain, you generate a public key. To be valid you have to
sign each block with your private key.

The details of the replication protocol are in the first two pages of the
Aether whitepaper. Personal blockchains are replicated peer-to-peer, so as
long as someone has subscribers, you can always get a copy of their blocks,
even if they are offline.

https://github.com/skycoin/whitepapers/raw/master/whitepaper_05_Aether.pdf

There are no coins on the personal blockchains and a personal blockchain
cannot trigger a transaction. So if you enter into a contract on a personal
blockchain its just an agreement, you still have to execute or perform under
the contract. However, if you dont perform, then its public. Or if only the
contract hash was published, no one knows what the contract is, but you can
publish the contract if they dont perform and everyone can see that.

The Bitcoin wire protocol only has one blockchain. The Skycoin wire protocol
has to support multiple blockchains. There is the main chain that has the coin
transactions for Skycoin. Then there are a number of personal blockchains you
are subscribed to. The way we handle this, is that there is a Skycoin Daemon
in Skywire. You create a "service" and associate it with the daemon. A
particular service instance might, replicate a blockchain for a given public
key. You create the service, configure it and then associate it with the
Daemon. This allows multiple blockchains to be synced using a single program
instance.

The Skywire daemon node has its own public key. The public key hashes to an
address that identifies the node. If you know the address, you will be able to
find the node, connect to it and send it messages. So there will be a
bitmessage sort of functionality built in.  There will be a very simple API
where you say SEND(Addr, "Hello, wuts up") and it handles finding the node and
sending the message.

Inter-node messaging is very useful, because if nodes can communicate, they
can generate new receiving addresses for each transaction. Merchants can send
invoices and receipts and applications can communicate securely.

So basicly, Skycoin has transaction addresses where you can send coins and it
has communication addresses, where you can send messages to.

This is all aimed at developers. The users wont see it until someone builds a
chat or email application on top of it. Many of the initial Skycoin bounties
will be for designing and developing these applications on top of the
infrastructure.
