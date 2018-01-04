+++
title = "CX Tutorial: Using Affordances to Build a Small Text-based Adventure"
tags = [
    "CX",
    "CX Tutorials",
    "Affordances"
]
bounty = 5
date = "2017-09-20"
categories = [
    "Tutorials",
]
+++

<!-- MarkdownTOC autolink="true" bracket="round" depth="2" -->

- [Introduzione](#introduzione)
- [Architettura reattiva](#architettura-reattiva)
- [Sistema di convenienza](#sistema-di-convenienza)
- [Oggetti](#objects)
- [Conclusione](#conclusion)

<!-- /MarkdownTOC -->

# Introduzione

Questo tutorial presenta un "gioco" basato sul testo (l'utente non interagisce
con il programma, e non può influenzare le decisioni del personaggio) che utilizza una
[Architettura reattiva] (#architettura-reattiva) a
determinare quali sono le possibili azioni che il personaggio del gioco può fare. 
Il codice sorgente completo può essere trovato in
[CX's repository] (https://github.com/skycoin/cx), nel file * examples / text-based-adventure.cx *.

Il gioco descrive l'avventura di un viaggiatore che fugge da un
mostro (Halloween arriverà il prossimo mese, dopotutto). Se il viaggiatore
sopravvive un certo numero di ore (beh, queste sono solo iterazioni in un loop *for*),
il mostro la smetterà di inseguire il viaggiatore. Un esempio di
una sessione di gioco è qui descritta:


```
Il viaggiatore continua a seguire il percorso, assicurandosi di ignorare qualsiasi dolore.
Ululando e ringhiando, il mostro sta arrivando.
Il coraggio entra in scena, nella speranza di vivere per un'altra notte.
Ingenuo, e anche stupido, ma l'atto del viaggiatore lascia intorpidito il mostro.
Nord Est Ovest Sud. Ogni direzione è buona,
finché non si trova nessun mostro.
Ululando e ringhiando, il mostro sta arrivando.
Il viaggiatore scappa via e la codardia lo lascia vivere per un altro giorno.

Sopravvivi.
```

Se il viaggiatore decide di combattere il mostro e il suo eroico tentativo
fallisce, il gioco finisce. Un esempio di fine del gioco è:

```
Nord, Est, Ovest, Sud. Ogni direzione è buona,
finché non si trova nessun mostro.
Ululando e ringhiando, il mostro sta arrivando.
Il coraggio entra in scena, nella speranza di vivere per un'altra notte.
Ma il fallimento descrive questa parvenza e, improvvisamente, quest'avventura finisce.

Muori.

Call's State:
flag:			true
nonAssign_32:		""

halt() Arguments:
0: "Muori."

65: call to halt
```

Come puoi vedere, viene generato un errore se muori (questo è appropriato,
è una situazione spaventosa per un programmatore).

# Archittettura reattiva

In questa architettura, viene posta una domanda e diversi agenti (in
questo caso, funzioni) devono rispondere a questa domanda. Una semplice domanda
quello che può essere chiesto è "Chi deve essere eseguito in questo momento?" e coloro
le cui funzioni che possono essere eseguite lo faranno.

I seguenti prototipi di funzioni rappresentano le possibili azioni che
possono accadere durante l'avventura del viaggiatore.

```
func walk (flag bool) () {}
func noise (flag bool) () {}
func consider (flag bool) () {}
func chance (flag bool) () {}
func fightResult (flag bool) () {}
func theEnd (flag bool) () {}
```

# Sistema di convenienza

Another function must coordinate the function calls. In this case,
CX's affordance system is used to determine if an action is allowed to
run or not.

```
yes := true
no := false

remArg("walk")
affExpr("walk", "yes|no", 0)
:tag walk;
walk(false)
```

In the code above, *remArg()* looks for an expression with the "walk"
tag and removes its argument. This is done in order to make the
affordance system list the arguments that can be sent to the
expression's operator. Next, *affExpr()* is telling CX "among all the
arguments that can be sent to *walk*, tell me if *yes* or no *no* can
be used as arguments, and apply the *0th* option from the affordance
list that you return."

The previous procedure is applied to all the actions that can happen
during the traveler's adventure. For each of these actions, the
following rules are queried to determine if the action should be
allowed or not:

```
setClauses("
          aff(walk, yes, X, R) :- X = monster, R = false.
          aff(noise, yes, X, R) :- X = monster, R = false.

          aff(consider, yes, X, R) :- R = false.
          aff(chance, yes, X, R) :- R = false.
          aff(fightResult, yes, X, R) :- R = false.
          aff(theEnd, yes, X, R) :- R = false.

          aff(consider, yes, X, R) :- X = monster, R = true.
          aff(chance, yes, X, R) :- X = fight, R = true.
          aff(fightResult, yes, X, R) :- X = fight, R = true.
          aff(theEnd, yes, X, R) :- X = died, R = true.
        ")
```

The first rule can be read as "I will be queried if you're considering
to send the *yes* argument to the *walk* action. If the object
*monster* is present, then this argument is *not* an option."

The rules in the second block (the 4 rules after the first empty line)
tell the affordance system to "never" accept a *yes* argument. We do
this because we want this to be the default behaviour, but we can
later declare rules that override this behaviour. This override
process happens with the last 4 rules. Basically, this block of rules
is telling CX to accept *yes* as arguments if a particular object is
present in the object stack.

# Objects

Some of the actions add or remove objects from the object stack. For
example, whenever the *noise* action decides to make the monster
appear, *addObject("monster")* is executed. If the traveler decides to
run away from the fight, the "monster" object is removed from the
stack.

In the case of the *chance* action, the monster can decide to spare
the traveler a few more seconds to see what he will decide to do
next. To do this, the "fight" object is removed (as the monster does
not want to start a fight yet), but the "monster" object remains.

# Conclusion

CX's affordance system uses objects and rules to make complex
decisions about how affordances are going to be filtered.

By using objects, we can decide what actions will be activated or
deactivated. For this example, a small amount of actions are being
considered for this activation process, and the benefit of using this
architecture could seem worthless at first sight. Nevertheless, more
complex rules involving more objects could be defined, and a single
rule could be in charge of activating several nodes in a big network
of actions. Also, in this example only two possible arguments are
considered: *yes* and *no*; we could have more arguments, and actions
that accept different types of arguments other than booleans.
