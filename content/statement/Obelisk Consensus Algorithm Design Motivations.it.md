+++
title = "Motivazioni per la progettazione dell'algoritmo di consenso Obelisk"
tags = [
    "Obelisk",
    "Consensus",
    "Skywire",
]
date = "2017-10-26"
categories = [
    "Statement",
]
bounty = 20
+++

*Questo è un post archiviato da una discussione sul forum BitcoinTalks del 19 giugno 2014*

>Citazione di: yxxyun del 19 Giugno, 2014, 02:52:38 AM

>>Citazione di: skycoin del 19 Giugno, 2014, 02:31:59 AM

>>>Citazione di: FrictionlessCoin del 18 Giugno, 2014, 09:15:07 PM

>>>>Citazione di: skycoin del 18 Giugno, 2014, 09:08:56 PM

>>>>Aggiornamento di sviluppo:

>>>>Abbiamo scoperto un modo per prevenire un attacco Sybil usando un sistema 
>>>>Proof of Stake ibrido.

>>>>Per creare un nodo, devi dimostrare di avere monete. Diciamo 10 monete. Invii 10 monete per l'indirizzo A. Quindi si inviano le 10 monete dall'indirizzo A all'indirizzo B. Poi aggiungi una firma usando la chiave pubblica nell'indirizzo A per firmare un messaggio nella blockchain di Obelisk.

>>>>In alternativa, è possibile pubblicare la chiave pubblica per l'indirizzo A e quindi firmare un messaggio con tale chiave pubblica. Il nodo dovrebbe pubblicare una firma ogni periodo di tempo, o ogni determinato numero di blocchi di valute di riserva che vengono spostati, per mantenere relazioni di fiducia valide con altri peer.

>>>>In alternativa, può essere richiesta una Proof of Burn (prova di ustione), quando le monete vengono inviate dall'indirizzo "A" all'indirizzo "B" che non ha una chiave privata. La Proof of Burn entra in conflitto con il requisito che nessuno dovrebbe scaricare l'intera blockchain dall'inizio per operare un intero nodo, quindi è improbabile.

>>>>Questo sistema superiore limita il numero di nodi Obelisk e limita la possibilità di eseguire nodi Obelisk ai possessori di monete. Il limite superiore del numero di nodi e i requisiti di moneta aggiungono un altro livello di protezione da un attacco Sybil.

>>>Non sono sicuro di come ciò prevenga un attacco Sybil. Stai semplicemente aggiungendo un costo per l'aggiunta di un nodo alla rete e quindi un attacco Sybil richiederebbe un costo finanziario per farlo?

>>È solo un'idea in questa fase. Abbiamo trovato un miglioramento. Ogni nodo in Obelisk ha una chiave pubblica. Facciamo l'hash della chiave pubblica in una direzione e si memorizzano 10 monete in un'uscita di proprietà di quell'indirizzo.

>>Non aggiunge un costo. Dimostra solo di possedere 10 monete. Dimostra che conosci la chiave privata, per una chiave pubblica, il cui indirizzo contiene 10 monete. Puoi ancora spendere le monete.

>>L'idea è che imposti un limite superiore al numero di nodi. Se si devono conservare 10 monete e ci sono 100 milioni di monete, si imposta un limite superiore sulla rete di 10 milioni di nodi. Il limite superiore non sembra matematicamente utile, per ora, ma è qualcosa che dovremmo prendere in considerazione.

>>Quando viene eseguito un nuovo nodo Obelisk, si "fiderà" di alcuni peer casuali. L'utente può anche aggiungere manualmente alcuni nodi di cui si fida (scambi o membri di una comunità fidata). Un nodo è identificato dall'hash della chiave pubblica e trovato da DHT. Non è come Bitcoin dove i nodi sono IP: coppie di porte. Puoi spostare il tuo computer e l'identità del nodo non dipende dal suo indirizzo IP.

>Vogliamo che la rete sia sicura con i nodi casuali scelti. Non vogliamo una situazione come Ripple, in cui i tre nodi degli sviluppatori controllano la rete. Tuttavia, volevamo evitare una situazione, in cui qualcuno gestisce 200.000 nodi e cerca di raccogliere le relazioni di fiducia dai nuovi utenti. Questi Sybil attaccano i nodi, non possono ancora attaccare il 51% in generale, ma tutto ciò che aumenta il costo dell'attacco è ancora utile.

>>Forse, lo limitiamo in modo tale che il nuovo utente possa fidarsi solo in modo casuale dei nodi che hanno un bilancio di monete. Le relazioni di fiducia non verranno interrotte se il nodo non ha un saldo di moneta, ma non otterranno nuovi utenti casuali..

>>Si presuppone che il grafico di connettività per le relazioni di trust sia un grafico collegato in modo casuale. Alcuni nodi (membri della comunità fidati, case di cambiamento, siti Web, organizzazioni) avranno più relazioni di fiducia e questo aiuta un po' al momento della convergenza per il consenso sui blocchi. Riduce leggermente il diametro della rete. Alcuni nodi saranno utilizzati per verificare il consenso (si scelgono un gruppo di Exchange o diverse chiavi pubbliche), tali nodi non influenzano le decisioni di consenso, ma sono "oramenti di consenso" per verificare se il proprio nodo è convergente con la rete.

>>Se due grandi Exchange hanno un consenso diverso per un particolare blocco, questo è un problema. Potrebbe indicare un netsplit o un attacco alla rete. Gli Exchange potrebbero voler sospendere il trading fino a quando il problema non sarà risolto.

>Obelisk è il nodo di consenso distribuito di skycoin? Pensavo che SkyCoind fosse il nodo ...

Si.

Skycoin ha una blockchain. La blockchain si trova qui:
https://github.com/skycoin/skycoin/tree/develop/src/coin. 
Questo analizza i blocchi e tratta gli output e le transazioni non spese.

Skywire è il demone e ha una "architettura di servizio". È in grado di eseguire servizi come la sincronizzazione blockchain e altro. La rete mesh è attualmente in fase di implementazione come servizio su skywire (anche se potrebbe dover cambiare).

l meccanismo di consenso è al di fuori della blockchain. I nodi Obelisk (che saranno probabilmente implementati come servizio Skywire) hanno una blockchain. Ogni nodo ha una chiave pubblica. La chiave pubblica identifica il nodo Obelisk. Ogni nodo Obelisk ha la sua blockchain (non ci sono monete in questa catena). Il nodo crea un nuovo blocco e lo firma con la sua chiave privata. Gli Obelisk blockchain sono usati per negoziare il consenso (determinando il blocco in cima nella blockchain Skycoin). Obelisk usa Ben-Or per un consenso casuale. Ogni nodo Obelisk ha una lista di altri nodi a cui si abbona. Quei nodi influenzano il consenso e le decisioni di voto per il nodo locale. Per le topologie di rete non patologiche, il consenso locale converge stabilmente in un consenso globale.

Ogni nodo vota sul prossimo blocco della catena. Un nodo propone il prossimo blocco e i nodi votano sul successore. I voti sono pubblicati nei blocchi nella blockchain di Obelisk per ciascun nodo. Il tuo nodo vota a caso tra l'alternativa e capovolge il suo voto di tanto in tanto. Una volta raggiunto il consenso del 40% dei tuoi peer (i nodi a cui sei iscritto), passa a quel candidato. La rete può votare su più biforcazioni contemporaneamente, non rallenta in attesa di un consenso. Le biforcazioni vengono potate in una singola catena nel tempo. Le divisioni di due o tre blocchi sono normali, ma dopo alcune conferme la probabilità che il blocco venga ripristinato diminuisce esponenzialmente fino a zero. Se una transazione è stata eseguita su tutte le catene candidate, viene essenzialmente eseguita, anche se la particolare catena di consenso non è stata ancora decisa.

That is binary Ben-Or's and Skycoin will use something slightly more advanced,
that is faster when there are multiple successor blocks to choose from in the
consensus set. Randomization is important to keep sub-graphs of the network
from getting stuck. The voting process is a form of "annealing" where each
node will arrive at the global consensus independently, only from its local
information.

The consensus process happens in public. A node publishes blocks, signs them
with their private key and the blocks are replicated peer-to-peer between
subscribers of the chain. Then there are "consensus oracles" which are nodes
that are used to verify consensus but do not influence consensus. So you might
choose the public keys of a few exchanges and a few trusted community members
and your node will use those to detect if something is wrong. This is used to
detect netsplits. This also protects against an attack, where a hacker
controls your router and can control the peers you are able to connect to.

If a node shows up to network and tries to get the network to accept a
different chain (51% attack, reverting transactions), it usually gets ignored.
Most 51% attacks require malignant node behavior which is automatically
detected and results in a subscribing node removing the malignant node from
their trust list. The easiest 51% attack strategy is easy to detect and prove
with mathematical certainty that it was intended as an attempt to revert
transactions, because it require backdating block consensus decisions.
It requires publishing two signed blocks with the same sequence number,
so we just made this an automatically bannable offence for a node.

We are trying to eliminate the last possible 51% attack, which is when a
subnetwork of nodes goes offline (netsplit attack) and then rejoins the
network with a different blockchain consensus and tries to force this on the
network to revert transactions. Most of these attacks will fail, because the
subnetwork will not have enough influence.

This attack is still very difficult to pull off. In case there is
a successful 51% attack, one solution is to freeze the network and let each node/user
individually choose which chain is the valid one and let people ban the attacking
nodes manually. The consensus oracle allows each node, with high probability, to
know if the state is synced and if global consensus has been reached or if
they are part of a netsplit subgraph. We think its possible for each node to
know with a high probability of correctness from local information, whether a
node was offline during a consensus decision and then ignore nodes that
were offline who suddenly appear and try to force a chain fork on the network.

In Bitcoin, if you have the most hashing power, you can revert transactions
whenever you want.

In Skycoin, to revert transactions:

- You much control a large number of nodes
- The nodes you control must be "influential" and trusted within the network
  topology
- Your nodes need to exhibit extremely blatant pathological attack behavior
  without the behavior being detected, because detection would result in losing
  the trust relationships you need to attack the network.
- Your nodes need to be in a pathological attack topology, without it being
  detected (most bot nodes will be trusted by very few humans and be very obvious)
- You must be able get the nodes you control to collude in a way that results
  in a successful attack (this is not very straightforward)
- If the attack succeeds, you must prevent the network from reverting the
  attack by hand (very difficult if people lost coins or money because of the
  attack)

To prove it iss 51% attack proof, you have to write down the assumptions you are
making and then create a simple mathematical model and then prove the
conditions under which things can and cannot happen in the model. Once you
know the conditions that an attack is possible under, you try to eliminate
them and if you cant eliminate them, you make them as difficult as possible.
You increase the cost of an attack and you reduce the probability that a
specific attack will succeed. Then you reduce the payoff and incentives for
the attack.

The consensus process is simple and easy to model, but unintuitive without
seeing it. There will be a javascript site eventually that has an animated
consensus process you can play with.
