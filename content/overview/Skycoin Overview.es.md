+++
title = "Visión general de Skycoin"
tags = [
    "Skycoin",
]
bounty = 15
date = "2017-08-26"
categories = [
    "Overview",
]
+++

<!-- MarkdownTOC autolink="true" bracket="round" depth="1" -->

- [Introducción a Skycoin](#introducción-a-skycoin)
- [Innovaciones e imperfecciones de Bitcoin y los protocolos actuales de cadena de bloques](#innovaciones-e-imperfecciones-de-bitcoin-y-los-protocolos-actuales-de-cadena-de-bloques)
- [Innovaciones producidas por Bitcoin](#innovaciones-producidas-por-bitcoin)
- [Defectos importantes de Bitcoin](#defectos-importantes-de-bitcoin)
- [Desirable Properties For Systems Of Distributed Consensus For Financial Ledgers](#desirable-properties-for-systems-of-distributed-consensus-for-financial-ledgers)
- [Skycoin Security Philosophy](#skycoin-security-philosophy)
- [Transparency And Security: Obelisk And Public Broadcast Channels](#transparency-and-security-obelisk-and-public-broadcast-channels)
- [Obelisk](#obelisk)
- [Simple Binary Consensus Algorithm: Choosing Between Two Blocks](#simple-binary-consensus-algorithm-choosing-between-two-blocks)
- [Consensus On Multiple Concurrent Branch Choices](#consensus-on-multiple-concurrent-branch-choices)

<!-- /MarkdownTOC -->

# Introducción a Skycoin

Skycoin se basa en una tecnología que presenta 
un nuevo fundamento criptográfico conocido como 
canal de transmisión público. También presenta 
una nueva implementación de algoritmo de consenso, 
llamado Obelisk, que minimiza los problemas de 
compromiso que surgen de la prueba de trabajo y 
de los procesos de minería subyacentes a Bitcoin, 
abordando una serie de asuntos de seguridad 
asociados con este último. Obelisk no es un simple algoritmo, 
sino una implementación que emplea múltiples 
técnicas para entregar garantías de seguridad específicas.

# Innovaciones e imperfecciones de Bitcoin y los protocolos actuales de cadena de bloques

En Bitcoin, las nuevas transacciones se colocan 
en un bloque, el cual se agrega a la cadena de 
bloques. Cualquier persona en la red de Bitcoin 
puede crear nuevos bloques. Por lo tanto, cada 
bloque tiene un único padre pero uno o más 
sucesores válidos (hijos). Las cadenas forman 
un árbol y el problema principal que Bitcoin 
resuelve es lograr que cada nodo de la red esté 
de acuerdo en cuál de las cadenas prospectivas 
en el árbol de cadena es la cadena de bloques
de consenso.

Bitcoin usa una técnica llamada Prueba de Trabajo 
(PoW) para determinar una cadena de bloques única. 
Un bloque válido requiere un valor hash, el cual está 
por debajo de un valor objetivo. Los nodos agregan 
transacciones a un nuevo bloque y aleatoriamente 
prueban "nonces" hasta que se encuentre un hash 
válido para un bloque.

Una función es usada para crear un orden total de 
cadenas en el árbol de bloques. La cadena que tiene 
la mayor dificultad y requirió la mayoría de las 
operaciones hash para producirse es "la cadena más larga" 
y forma la cadena de consenso. La noción de 
"profundidad de bloque" y "dificultad" crea un 
orden total sobre todas las cadenas lineales en el 
árbol de bloques y solo la cadena que requiere más 
recursos es aceptada para producir la cadena de consenso.

Los nodos de Bitcoin se conectan entre sí al azar y cada 
nodo retransmite la cadena de bloques más difícil que 
conoce a sus pares. Si un nodo tiene una cadena más difícil 
de producir que otro nodo conectado, el otro nodo recibirá 
los bloques secuencialmente. El nodo evaluará la función y 
decidirá si la cadena recibida es más difícil de producir y, 
por lo tanto, puede cambiar su consenso a la cadena recibida y 
luego anunciará su nueva cadena a sus pares. De esta forma, 
el consenso se propaga a través de la red y todos los nodos 
obtienen el mismo consenso.

Bitcoin no asume que los nodos tengan identidades 
y no asume que los nodos son honestos. Los nodos pueden 
enviar datos a otros nodos y no pueden afectar las decisiones 
consensuadas porque la dificultad es algo que puede verificarse 
independientemente por sus propios méritos.

# Innovaciones producidas por Bitcoin

### * La cadena de bloques

Una estructura de datos única que todos pueden poseer.

### * Registro público de las transacciones 

Almacenamiento de transacciones financieras en la cadena de bloques.

### * Uso de PoW y del reajuste de dificultad para mantener una tasa constante de producción de bloques

### * Uso de hashes de llave pública como direcciones

Las llaves públicas no son reveladas hasta que se usan.

### * Uso de "outputs" (salidas) para los saldos

Se ignora tratar de crear efectivo digital divisible: 
para pagar $20 de una salida de $25, se envían $20 a 
la persona y $5 a usted mismo.

### * Funcion de dificultad de PoW y profundidad de bloque

Primer uso de una función que define el orden total en 
árboles de bloques. El registro contable público elude 
el problema del doble gasto del efectivo digital tradicional.

# Defectos importantes de Bitcoin

Estos son los problemas que deben abordarse en el 
desarrollo de nuevas criptomonedas. Bitcoin debe 
considerarse como una criptomoneda embrionaria sobre 
la cual los desarrollos futuros deben mejorar. La 
tecnología en la que se basa Skycoin responde a las 
principales deficiencias de Bitcoin rediseñando todo 
el sistema de consenso distribuido.

### * Las decisiones de consenso en Bitcoin no son definitivas y pueden revertirse

Una persona u organización que tenga la 
posibilidad de alquilar o comprar 
suficiente potencia hash puede revertir las transacciones.

### * Bitcoin logra el consenso de red pero los nodos 
individuales de Bitcoin son altamente vulnerables a los 
adversarios que controlan los routers a través de los cuales pasan los paquetes

Un adversario que controle los routers tiene 
dominio absoluto sobre la opinión de un nodo y puede 
influir arbitrariamente en las decisiones de consenso del nodo.

### * Las casas de cambio de Bitcoin se han vuelto altamente vulnerables a los ataques

Los atacantes hábiles pueden emplear ataques del 
51%  y la compra y venta de monedas alternativas en 
los mercados de Bitcoin para conducir a este a la insolvencia.

### * Los bancos y los sitios de apuestas se han vuelto vulnerables a los ataques del 51%

### * A medida que Bitcoin madura, la compra de opciones contra Bitcoin y los ataques contra las redes se vuelven más rentables

En el futuro, los ataques exitosos contra 
Bitcoin podrían generar varios cientos de 
millones de dólares en ganancias por el 
comercio de opciones.

### * Los estados con fuertes controles de capital, 
al igual que las empresas competidoras, 
pueden atacar directamente a la red de Bitcoin para 
proteger sus intereses financieros

Tales entidades pueden absorber fácilmente 
los costos de atacar la red y acabar con la seguridad de Bitcoin.

### * Los servicios que permiten "Hashing en la nube" y el alquiler de Hash Power de terceros son cada vez más exitosos

Muchos ahora tienen la capacidad de alquilar 
el poder de hash para un ataque mayoritario.

### * Los hackers pueden utilizar numerosos agujeros de seguridad en routers y equipos de 
redes para robar monedas de bancos y casas de cambio

Un atacante puede controlar a los pares 
conectados a un nodo de Bitcoin y garantizar 
conexiones a los nodos que son controlados 
por el atacante. Por ejemplo, un atacante 
puede introducir una transacción de depósito 
en la cadena lateral de un banco y lograr que 
el banco emita una transacción de retiro que 
luego se retransmite a la red principal.

### * Bitcoin no puede ofrecer seguridad a un bajo costo 

La red de Bitcoin usa cantidades inmensas 
de electricidad que son exponencialmente 
crecientes. La seguridad de Bitcoin se basa 
deliberadamente en la creación de la mayor 
cantidad posible de desechos eléctricos. Como 
la seguridad está relacionada con el costo de 
lograr la tasa de hash mayoritaria, el costo 
de funcionamiento de la red de Bitcoin aumenta 
constantemente. En un sistema bien diseñado, $1 
en seguridad costaría $1000 para eludir. Pero en 
Bitcoin, la proporción es de $1 a $1. Además, 
esto es ambientalmente irresponsable.

### * Bitcoin no puede disminuir los tiempos de transacción sin comprometer la seguridad

Las transacciones de Bitcoin tardan 
en promedio 10 minutos en incluirse en 
un bloque, y se necesita aun más tiempo para
obtener más seguridad.

# Cualidades convenientes para los sistemas de consenso distribuido para los registros financieros

Los parametros sobre los cuales se puede mejorar Bitcoin son:

### * Eliminar el gasto doble

Una vez que se ha ejecutado una transacción, 
debería ser imposible revertir el consenso. 
este debe ser lo más irreversible posible.

### * Eficiencia

El costo de llevar un registro contable 
perfectamente seguro debería ser extremadamente bajo. 

### * Velocidad

El sistema debería posibilitar que las transacciones 
se confirmen en segundos.

### * Transparencia

Debería ser fácil auditar e identificar nodos maliciosos.

### * Seguridad para ataques de router

Los nodos deberían ser capaces de 
detectar si su consenso difiere de la red.

Algunas propiedades de seguridad deberían permanecer 
intactas, incluso si la gran mayoría de los nodos en la red son maliciosos.

A nivel fundamental, muchos de los defectos de seguridad 
asociados con el sistema de Bitcoin surgen del problema 
de compromiso inherente en la prueba de trabajo y en los 
procesos de minería. Sus defectos de seguridad representan 
un problema de los generales bizantinos en el mundo real. 
Existen incentivos para que los participantes manipulen 
los procesos de verificación, a través del soborno y el 
hackeo, por ejemplo. Los atacantes manipularán los relojes 
del sistema, comprometerán los routers, usarán colisiones 
hash, inundarán la red con cientos de miles de bots y 
explotarán la maleabilidad de la firma.

Un sistema seguro no solo debe ser capaz de protegerse 
contra cada ataque conocido, sino que debe ser lo 
suficientemente robusto como para evolucionar y adaptarse 
a futuros ataques. Algunos problemas en Bitcoin pueden 
corregirse, como la maleabilidad de la firma. Otros 
problemas son fundamentales y no pueden abordarse sin 
definir una estructura completamente nueva, como la 
dependencia en la Prueba de trabajo y los mineros.

# Filosofía de seguridad de Skycoin

La seguridad es un proceso de identificación y 
fortificación continua contra las amenazas. Un buen 
sistema logra una "defensa profunda", tiene múltiples 
sistemas redundantes y sobrevivirá a la falla completa 
de cualquier medida individual. La buena seguridad 
requiere la diferenciación entre las amenazas que son 
existenciales y las que son simples molestias.

A pesar de que es obvio que ningún sistema puede 
eliminar todas las amenazas de seguridad y alcanzar 
simultáneamente todos los objetivos enumerados 
anteriormente, Skycoin representa el siguiente paso 
en la tecnología de criptomoneda porque adopta un 
enfoque modular en capas para la seguridad y utiliza 
diferentes sistemas para aplicar garantías 
particulares. La seguridad de Skycoin se centra 
en abordar las amenazas existenciales que enfrenta 
Bitcoin y proteger a los usuarios de las amenazas 
cotidianas, intentando brindar el más alto grado 
de protección contra la clase de ataques que causarían 
las mayores pérdidas a sus usuarios, inversionistas e 
instituciones. Esto requiere un rediseño completo de 
Bitcoin en ambos extremos, desde la generación de 
cartera hasta el consenso de la cadena de bloques, y 
la innovación fundamental en muchas otras áreas.

La mayoría de las pérdidas en Bitcoin derivan de 
deficiencias en el diseño, la falta de funcionalidad 
y los errores del usuario final en lugar de ataques 
técnicos al software o matemáticas. 
Skycoin debe abordar tanto las inminentes amenazas 
matemáticas existenciales como los peligros de 
seguridad que la experiencia de usuario incompleta 
y poco pensada de Bitcoin ha creado para los usuarios 
cotidianos. La mala funcionalidad y el diseño han 
forzado a los usuarios a comprometer la seguridad, 
con millones de dólares confiando rutinariamente en 
carteras web inseguras. A pesar de los robos frecuentes 
y masivos reportados por los medios a diario, hasta la 
fecha se han perdido más Bitcoin debido a problemas de 
funcionalidad que de todos los esfuerzos de los delincuentes 
para robar Bitcoin.

Alrededor de la mitad de todos los Bitcoins 
existentes nunca se han movido de sus direcciones 
iniciales y nunca lo harán porque están perdidos 
como archivos de cartera irrecuperables, carteras 
olvidadas, o malentendidos de lo que en realidad se 
estaba respaldando en un archivo de cartera. Mt. Gox 
informó recientemente haber "encontrado" 200,000 Bitcoin 
en una cartera que no sabían que contenía bitcoins. La 
cartera había sido ignorada previamente y pudo haber sido 
borrada fácilmente por error. Las carteras a menudo se 
confunden como vacías porque el software de la computadora 
no puede cargar una cartera creada por un 
software "demasiado viejo". Por lo tanto, la mayoría 
de los problemas de seguridad relacionados con Bitcoin 
ocurren a nivel de funcionalidad, usuarios finales y 
seguridad de las casas de cambio.

El resto de esta sección cubre algunas de las nuevas 
técnicas que hemos creado en cooperación con nuestros 
socios para abordar los problemas de seguridad a nivel 
de red y hacer que la cadena de bloques de Skycoin sea 
más segura que las redes anteriores.

Hemos demostrado matemáticamente que nuestro 
sistema logra consenso, tiene las propiedades 
de seguridad que queremos y funciona bien en 
condiciones de red normales. Tenemos algunas 
nuevas estructuras de datos emocionantes que 
no se han visto antes en ninguna moneda o pieza 
de software. Por el momento, estamos creando un 
prototipo del sistema para su implementación. 
El proceso de desarrollo de Skycoin es iterativo. 
Habrá cambios, mejoras y mejoras a medida que 
trabajamos a través de los detalles, abordamos 
fallas conocidas, probamos el sistema y obtenemos retroalimentación.

# Transparencia y seguridad: Obelisk y Canales de transmisión pública

Para resolver los problemas de compromiso asociados 
con el sistema de Bitcoin, la tecnología subyacente a 
Skycoin implementa la cadena de bloques en la forma de 
un canal de transmisión pública. Todos pueden leer la 
cadena, pero solo el propietario puede publicar bloques. 
Para ser válida como una cadena personal, cada bloque 
debe estar firmado con la llave privada de los 
propietarios. Cada nodo en este sistema de algoritmo 
de consenso (Obelisk) tiene una cadena de bloques 
personal, este es el núcleo principal del sistema Obelisk.

El canal de transmisión pública impone varias limitaciones:

### * Una vez un bloque es publicado, este no puede ser removido

Los bloques se replican de igual a igual 
a todos los suscriptores. Una vez que se 
ha publicado un bloque, se propaga a todos 
los suscriptores. Debes destruir a todos 
los pares que han recibido el bloque para 
lograr borrarlo de internet.

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
