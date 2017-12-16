+++
title = "Skycoinの概要"
tags = [
    "Skycoin",
]
bounty = 30
date = "2017-08-26"
categories = [
    "Overview",
]
+++

<!-- MarkdownTOC autolink="true" bracket="round" depth="1" -->

- [Skycoinの紹介](#skycoin-introduction)
- [Bitcoinと現在のブロックチェーンプロトコルによる革新と欠陥](#innovations-and-flaws-with-bitcoin-and-the-current-blockchain-protocols)
- [Bitcoinが起こした革新](#innovations-produced-by-bitcoin)
- [Bitcoinの主な欠陥](#major-flaws-of-bitcoin)
- [金融台帳の分散合意システムに求められる特性](#desirable-properties-for-systems-of-distributed-consensus-for-financial-ledgers)
- [Skycoinのセキュリティ哲学](#skycoin-security-philosophy)
- [透明性とセキュリティ: オベリスクと公開ブロードキャストチャンネル](#transparency-and-security-obelisk-and-public-broadcast-channels)
- [オベリスク](#obelisk)
- [シンプルバイナリ合意アルゴリズム: ２つのブロックからの選択](#simple-binary-consensus-algorithm-choosing-between-two-blocks)
- [ブランチ選択肢が複数ある場合の合意形成](#consensus-on-multiple-concurrent-branch-choices)

<!-- /MarkdownTOC -->

# Skycoinの紹介

Skycoinは、公開ブロードキャストチャンネルと呼ばれる新しい暗号要素を使った技術に基づいています。
オベリスクと呼ばれる新しい合意形成アルゴリズムの実装も導入されています。
Bitcoinの基礎となっているProof-of-Workとマイニングプロセスに起因するコミットメント問題を緩和し、
これに関連するセキュリティ上の問題に対応します。
オベリスクは単一のアルゴリズムではなく、いつかのセキュリティ上の保証を実現するために複数の手法を採用した実装です。

# Bitcoinと現在のブロックチェーンプロトコルによる革新と欠陥

Bitcoinでは、新しいトランザクションがブロックに配置され、ブロックチェーンに追加されます。
Bitcoinネットワーク内のすべてのピアは、新しいブロックを作成できます。
したがって、各ブロックが持つ親は1つですが、有効な後継子（子）は1つ以上になり得ます。
チェーンがツリーを形成しますが、Bitcoinは、合意チェーンがどれであるかを、
ネットワーク内の全てのノードがどのように合意するのかという重要な問題を解決します。

Bitcoinは、Proof-of-Work（PoW）という手法を使用して、一意のブロックチェーンを決定します。
有効なブロックは、そのハッシュ値が目標値以下である必要があります。
ノードは新しいブロックにトランザクションを追加し、
ブロックのハッシュ値が目標値以下になるまで、ナンスをランダムに変えて試します。

関数を使用して、ブロックツリー内の合計連鎖次数が計算されます。
最も難易度が高く、それを生成するために必要なハッシュ計算が最も多いチェーンは、「最長チェーン」であり、合意チェーンとなります。
「ブロックの深さ」と「難しさ」の概念から、ブロックツリーのすべてのチェーンについて合計連鎖次数が計算され、
生成するために最も計算リソースを消費するチェーンのみが合意チェーンとして受け入れられます。

Bitcoinノードはランダムに相互に接続し、各ノードは、それぞれが知っている最も難しいチェーンをピアに中継します。
もしあるノードが他の接続されたピアより難しいチェーンを持っている場合、他のピアはそのチェーンを順番に受信します。
ピアは関数を使用し、受け取ったチェーンがより難しいものであることを確かめ、
そのチェーンが最長チェーンであることに合意するかどうかを決めるでしょう。
ピアは、新しいチェーンを他のピアに配信します。
このようにして、合意がネットワーク全体に広がり、すべてのノードが同じ合意に達します。

Bitcoinは、ノードがアイデンティティを持っていると仮定せず、ノードが正当であるとも仮定しません。
ノードは他のノードに自由にデータを送信できますが、難易度は各ノードが独立して検証できるため、合意形成に影響を与えることはできません。

# Bitcoinが起こした革新

### * ブロックチェーン

誰もが持つことができる単一のデータ構造。

### * 取引のための公開台帳

ブロックチェーンに金融取引を格納する。

### * PoWの利用と、一定のブロック生成速度を維持するための難易度調整

### * アドレスとしての公開鍵ハッシュの利用

公開鍵は使用するまで公開されません。

### * 残高整合性を保つための「アウトプット」の利用

分割可能なデジタル通貨を作ろうとする必要はありません。
$25のアウトプットから$20を払うためには、$20を送り$5を自分自身に送ります。

### * PoW難易度関数とブロック深度

ブロックツリーの合計連鎖次数を定義する関数の使用。
公開台帳は、従来のデジタル通貨の二重支出問題を回避する。

# Bitcoinの主な欠陥

これらは、新しい暗号通貨の開発で取り組まなければならない課題です。
Bitcoinは、今後の開発によって改善する必要がある、原始的な暗号通貨と見なされるべきです。
Skycoinの基盤となるテクノロジーは、分散合意システム全体を再設計することで、Bitcoinの大きな欠点に対処します。

### * Bitcoinの合意は最終決定ではなく、元に戻すことができます

十分なハッシュ計算力をレンタルまたは購入できる人や組織は、トランザクションを元に戻すことができます。

### * Bitcoinはネットワーク上で合意を形成しますが、個々のBitcoinノードは、パケットが通過するルータを制御する攻撃に対して非常に脆弱です

ルータを制御する攻撃者は、ノードの視野を制御し、ノードの合意決定に任意に影響を与えることができます。

### * Bitcoinの取引所は、攻撃に対して非常に脆弱になっています

巧みな攻撃者は、Bitcoin取引所で51％攻撃とアルトコインの売買を行い、取引所を破産に追い込むことができます。

### * 銀行とギャンブルのサイトは51％攻撃に対して脆弱になっています

### * Bitcoinが成熟するにつれて、Bitcoinに対抗するオプションの購入とネットワークに対する攻撃がより利益的になっています

将来、Bitcoinに対する攻撃が成功すれば、オプション取引の利益は数百億ドルになる可能性があります。

### * 競争力のある企業と同様に、強い資本規制力を持つ州は、金融利益を守るためにBitcoinネットワークを直接攻撃するかもしれない

このような存在は、ネットワークの攻撃コストを負担して、Bitcoinのセキュリティを損なう可能性があります。

### * サードパーティが提供する「クラウドハッシュ計算」と計算パワーのレンタルサービスがますます成功している

多くの大規模プールは、ハッシュ計算パワーをレンタルする機能があり、51%攻撃に使われる恐れがあります。

### * ハッカーは、銀行や取引所からコインを盗むために、ルータやネットワーク機器にある数多くのセキュリティホールを使用することができます

攻撃者は、Bitcoinノードに接続されているピアをすり替え、攻撃者が制御するノードに接続させることができます。
例えば、攻撃者は、銀行側のブロックチェーンに偽の預け入れトランザクションを記録した後、
銀行に引き出しトランザクションを発行させ、それをメインネットワークに中継することができる。

### * Bitcoinは低コストでセキュリティを提供できない

Bitcoinネットワークは、膨大な電力を指数関数的に使用しています。
Bitcoinのセキュリティは、できるだけ多くの電気を無駄にすることに意図的に依存しています。
セキュリティは、過半数のハッシュ計算力を達成するコストに保証されているため、
Bitcoinネットワークを実行するコストは常に上昇します。
うまく設計されたシステムでは、$1が費やされたセキュリティを破るためには$1000かかります。
Bitcoinでは、この比率は$1対$1です。さらに、これは地球環境に対して無責任です。

### * Bitcoinは基本的に、セキュリティを損なうことなく決済にかかる時間を短縮できない

Bitcoinトランザクションは、ブロックに含まれるまでに平均10分かかるため、
セキュリティを強化するためにはさらに時間がかかります。

# 金融台帳の分散合意システムに求められる特性

Bitcoinを改善できる観点は次のとおりです。

### * 二重支払なし
一度トランザクションが実行されると、合意形成を元に戻すことは不可能です。
合意形成は可能な限り不可逆的でなければなりません。

### * 効率

完璧に安全な台帳を実行するコストは、非常に低くすべきです。

### * 速度

システムは、トランザクションが数秒以内に確認されるようにする必要があります。


### * 透明性

悪意のあるノードを調査して識別することは容易でなければなりません。

### * ルータ攻撃のセキュリティ

ノードは、自身の合意がネットワークと異なるかどうかを検出できるはずです

ネットワーク内のノードの大部分が悪意を持ち結託していても、
一部のセキュリティー・プロパティーはそのまま残るはずです。

基本的なレベルでは、Bitcoinシステムに関連するセキュリティ問題の多くは、
プルーフ・オブ・ワークとマイニングプロセスに起因するコミットメント問題から発生します。
そのセキュリティ問題は現実のビザンチン将軍問題を表しています。
参加者には検証プロセスを改ざんする動機（贈収賄やハッキングに関与するなど）が存在します。
攻撃者はシステムクロックを操作し、ルーターを危険にさらし、ハッシュ衝突を使用し、
数十万のボットでネットワークを氾濫させ、トランザクション展性を悪用します。

安全なシステムは、あらゆる既知の攻撃から保護するだけでなく、
将来の攻撃に対応して進化し適応するように十分に堅牢でなければなりません。
トランザクション展性など、Bitcoinのいくつかの問題を修正することができます。
プルーフ・オブ・ワークやマイニングへの依存など、その他の問題は根本的なものであり、
まったく新しいフレームワークを定義しなければ対処できません。

# Skycoinのセキュリティ哲学

セキュリティは、脅威に対する継続的な識別と防御のプロセスです。
優れたシステムは「徹底的な防御」を達成し、複数の冗長システムを持ち、
個々の対策のどんな失敗からも生き残ります。
よいセキュリティは、実害のある脅威と単なる迷惑な脅威との区別を必要とします。

単一のシステムがすべてのセキュリティ脅威を排除し、
同時に上に挙げたすべての目的を達成することはできないことは明らかです。
Skycoinは、セキュリティのためにモジュールレイヤー化されたアプローチを取り、
特定のセキュリティ保証を実現するために複数の異なるシステムを使用するため、
次世代の暗号通貨技術であると言えます。
Skycoinのセキュリティは、Bitcoinが直面する実在の脅威に対処し、
ユーザー、ステークホルダー、および機関投資家に最大の損失を与えるような攻撃に対して
最高レベルの保護を提供することを試み、日々の脅威からユーザーを保護することに重点を置いています。
これには、ウォレットの生成からブロックチェーンの合意形成までの全ての範囲におけるBitcoinの完全な再設計と
その他のいくつかの領域で根本的な革新が必要です。

Most of the losses in Bitcoin derive from
deficiencies in design, a lack of usability, and end user
mistakes rather than fundamental technical attacks in
the software or mathematics. Skycoin must address
both the looming existential mathematical threats
and the security perils that Bitcoin’s incomplete and
poorly thought out user experience has created for
everyday users. The poor usability and design has
forced users to compromise security, with millions
of dollar routinely relying on insecure web wallets.
Despite the frequent and massive thefts reported by
the media on a daily basis, to date more Bitcoin have
been lost due to usability issues than all the efforts of
criminals to steal Bitcoin.

As many as half of all existing Bitcoins have
never been moved from their initial addresses and
never will be because they are lost as unrecoverable
wallet files, lost wallets, or misunderstandings of
what was actually being backed up in a wallet file.
Mt. Gox recently reported “finding” 200,000 Bitcoin
in a wallet they were unaware carried Bitcoins. The
wallet had been previously ignored and could have
easily been deleted by mistake. Wallets are frequently
mistaken as empty because computer software was
unable to load a wallet created by software that is “too
old”. Thus most security issues concerning Bitcoin
occur at the level of usability, end users and exchange
security.

The rest of this section covers some of the new
techniques we have created in cooperation with our
partners to address network level security issues and
render the Skycoin blockchain more secure than
previous networks.

We have proven mathematically that our system
achieves consensus, has the security properties we want
and operates well under normal network conditions.
We have some exciting new data‐structures that have
not been seen in any coin or piece of software before.
At the moment we are prototyping the system for
deployment. The Skycoin development process is
iterative. There will be changes, improvements and
refinements as we work through the details, address
known flaws, test the system and get feedback.

# Transparency And Security: Obelisk And Public Broadcast Channels

To address the commitment problems associated
with the Bitcoin system, the technology underlying
our Skycoin implements the blockchain in the form
of a public broadcast channel. Everyone can read
the chain, but only the owner can mint blocks for it.
To be valid for a personal chain, each block must be
signed by the owners private key. Each node in this
consensus algorithm system (Obelisk) has a personal
blockchain and it is the core primitive in the Obelisk
system.

The public broadcast channel imposes several
constraints:

### * Once A Block Is Published, It Cannot Be Unpublished

Blocks are replicated peer to peer to
all subscribers. Once a block has been
published, it spreads to all subscribers.
You have to destroy all peers who have
received the block to erase it from
internet.

### * A Node Cannot Publish A Different Version Of An Earlier Block Without Detection

Blocks are numbered and it would
be detected if the node signed two
different blocks with the same
sequence number.

### * A Node Cannot Backdate The Timestamp On The Receipt Of A Block, Without Delaying The Publication Of A Block

Timestamps only go up, timestamps
increase monotonously with block
sequence count.

### * A Block In The Middle Of The Chain Cannot Be Changed Without Invalidating Every Block That Comes After It

In a hash chain, each block header contains
a hash of the previous block.

# Obelisk

Each Obelisk node (Skycoin Consensus Node)
has a public key (an identity) and personal blockchain
(a public broadcast channel). Consensus decisions
and communication happen within the personal
blockchains of each Obelisk node. This is a public
record of everything a node does. This allows the
community to audit nodes for cheating and collusion.
It gives the community a way to identify nodes which
are participating in attacks on the network and
it makes public how decisions in the network are
being made and which nodes are influencing those
decisions.

Each node has a list of other nodes that it
subscribes to. Nodes with more subscribers are more
“trusted” and yield more influence in the network. If
the community does not trust the nodes representing
them or feels that power within the network is too
concentrated (or not concentrated enough) the
community is able to collectively shift the balance of
power in the network by collectively changing their
trust relationships in the network.

Node subscription relationships can be
random and/or can be formed through web of trust
(subscribe to nodes of people you know and people
in the community you trust).

When a node receives a new block from a chain
it is subscribed to, it publishes the hash of the block
it publishes. This is a public acknowledgment of the
receipt of the block. Each block is timestamped
and counter-references blocks from other chains.
This creates a dense interlinked chain of block
acknowledgments. These chains establish causal
relationships and can act as a distributed time
stamping system as described in the next section.
This allows the network to prove that data did not
exist or was not published to the network or establish
that particular nodes were active or offline during a
particular time interval.

The current Obelisk consensus algorithm
is based upon Ben‐Or’s randomized consensus
algorithm.

A Sybil attack in a random graph (worst case)
allows the Sybil nodes to control consensus, but the
nodes are unable to revert transactions, removing the
only economic incentive to attack the network. In real
world graphs the Sybil resistance of the network is
actually very high and running a node is moderately
costly in terms of bandwidth, which makes large
botnets prohibitive.

Trust relationships are scarce and can be
rescinded. In the event of an attack, the network
reacts by severing connections to less trustworthy
nodes and contracting to a smaller core of trusted
nodes. The public record left by each node’s personal
blockchain makes it very easy to identify the nodes
participating in an attack. As attacking nodes are
identifed, individuals sever relationships with those
nodes, reducing their influence. Therefore, the major
benefits of the Skycoin network are:

- Skycoin consensus is democratic and nodes are run by the community
- Skycoin node consensus is public
- Every node is accountable to the community and 3rd party audits
- Influence within the skycoin consensus system is democratic and transparent (but unequal)

# Simple Binary Consensus Algorithm: Choosing Between Two Blocks

Each voting decision is a hash pair (A,B). A is
the hash of the parent of the block and B is the hash of
the block. Each node votes on the next block
it believes should be the consensus block. If 40% of
the nodes it is subscribed to have the same candidate
for consensus, the node changes its consensus to that
block. The node flips randomly between candidates
until consensus is reached.

# Consensus On Multiple Concurrent Branch Choices

A more advanced system publishes (A,B,P),
where P is a value from 0 to 1. P values across all
successors to block would sum to 1. This allows for
concurrent consensus decisions on multiple chain
branches.

If the majority of nodes in the network are
honest, they will also converge to the same consensus.

Skycoin also has a limited form of Proof of
Stake. We bias voting in favor of blocks with a larger
transaction fee.

If there are only two possible consensus
choices for a given parent and both blocks execute
your transaction, then the transaction is effectively
executed regardless of which of the two blocks end up
chosen by the network. The probability of reversion
of an early consensus decision declines exponentially
with block depth.
