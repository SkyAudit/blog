+++
title = "Bitcoinが目指した世界の実現のために"
tags = [
    "Decentralization",
    "Bitcoin",
    "Transaction Malleability",
    "Consensus",
]
bounty = 8
date = "2017-10-03"
categories = [
    "Statement",
]
+++

*本記事は「the bitcointalks」の投稿（２０１４年４月２３日）をアーカイブしたものです。 

> 引用: **JFK01** 2014年4月23日 01:54:50 PM

> Skycoinは検証に検証を重ね、とても慎重にプロジェクトを進めているように私には映ります。 
それ自体は悪いことではありません。Skycoinのチームはエンジニアであり、実践よりも検証を優先したいのでしょう。
とても注意深く開発も進められてきたようです。エンジニアは注意深く、慎重であるのが常です。
そのようにすればよいと思います。これは野心的なプロジェクトで、実現もまだまだ先の話でしょうから。

Peer Coin、Ripple、Bitcoin、Nxt、Litecoin、現在（２０１４年当時）人気の仮想通貨を見てみると、
どれもBitcoinのセキュリティモデルを根本的に改善することに成功しています。

- Bitcoinは仮想通貨の起源であり、Proof of Workの仕組みを導入しました。
- LitecoinはScryptという新たなハッシング機能を開発しました。
- Peer CoinはProof of Stakeを導入した最初の仮想通貨です。
- Ripple introduced a model of relational consensus, which is flawed but innovative. Has a new codebase.
- Nxt introduced pure proof of stake. Has a new codebase.
- Namecoin introduced a key value store on the blockchain.

Market caps:

- Bitcoin: 6 billion
- Litecoin: 300 million
- Peercoin : 50 million
- Ripple: 1 billion (but users have less than 1% of coins; so free float is only 50 million)
- Nxt: 25 million
- Namecoin: 20 million

Skycoin has a fundamentally new consensus model that is a major advance by
itself.

- Transactions in seconds.
- Transactions cannot be reverted by people who control hashing power.
- Low cost to run network.

Just the consensus model by itself will do well. A major attack affecting
Bitcoin exchanges or abuse by miners could move people away from PoW towards
coins like Skycoin that guarantee against attacks that revert transactions.

We knew about signature malleability three years ago and a decision was made
not to fix the problem in Bitcoin, until people began using it to steal coins.
We know about several attacks on Bitcoin that are theoretical, but which are
becoming more likely and profitable as the coin value goes up. If you can use a
router exploit and hack an internet router that traffic for a Bitcoin node goes
through, you can fork the network for that node and steal coins from exchanges,
banks and gambling sites.

Skycoin is hardened against all known vulnerabilities in Bitcoin. We were
immune to signature malleability before the attacks on Bitcoin even began. We
check for hash collisions from the start and did not have to monkey patch it.
If someone develops a widespread SHA256 preimage attack, Bitcoin is dead and
Skycoin will probably still be running.

Banks and businesses do not want to learn about or check for Signature
Malleability. They dont want to know how Bitcoin works. They want to be able to
trust that the developers fix known vulnerabilities before coins get stolen.
Bitcoin is not clean or mature software. Fixing many of the minor issues in
Bitcoin require a blockchain reset, that the developers are unable and
unwilling to do.

With the financial success of Bitcoin, we have forgotten that Bitcoin was first
and foremost an experiment. Many of the earliest developers of Bitcoin were
selling massive blocks of coins when the price hit a dollar, because they never
believed it would even get that far. People talk about Bitcoin 2.0, but we
should be trying to get to Bitcoin 1.0 . No one can look at the Bitcoin-qt
wallet and tell me that this is the best we can do.

*In other words, the first goal of the Skycoin project was not to surpass Bitcoin, but merely to get to where we believe Bitcoin should be.*
