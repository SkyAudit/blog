+++
title = "Skywire y Viscript"
tags = [
    "Development",
    "Skywire",
    "Viscript",
]
bounty = 3
date = "2017-08-15"
categories = [
    "Skywire",
]
+++
## Introducción

### Viscript

[Viscript](https://github.com/skycoin/viscript) es una interfaz de 
línea de comandos multiplataforma, y un iniciador de aplicaciones 
y eventualmente un administrador de clúster. Está basado en la biblioteca 
"signal" como un servidor "signal", por lo que puede gestionar 
los clientes de "signal", como el nodo y los componentes en skywire. 
Se puede ejecutar con interfaz grafica o sin ella. 

#### Captura de pantalla de la interfaz gráfica de Viscript:

![screenshot](viscript.jpeg)

Podemos agregar configuraciones de aplicaciones en el archivo config.yaml, como el meshnet-socks-server:  

```
  meshnet-socks-server:
    daemon: true
    desc: DESCRIPTION GOES HERE
    path: bin/meshnet/meshnet-run-socks-server
    default_args: []
    help: |
        [1] Text name of app, must be unique
        [2] Node address which app will be talked with. ex 101.202.34.56:9000
        [3] Port which socks server will use for connecting with target host. ex 8000

        Full Example Command:
            start meshnet-socks-server sockssrv0 101.202.34.56:9000 8001
```

Después de reiniciar Viscript, podemos verificar las aplicaciones que pueden ser iniciadas por Viscript, con el comando apps

Como se puede ver en la captura de pantalla, podemos iniciar la aplicación usando el comando `s` (`s apptracker 127.0.0.1:20000`).

Luego, Viscript lo inicia con un id único de secuencia, 
podemos hacer ping(`ping`), verificar el uso de recursos(`ru`)
y apagarlo(`sd`) a través de este id.

### Skywire

[Skywire](https://github.com/skycoin/skywire) es una red 
alternativa de pares que toma el control de los ISP y se 
lo devuelve a los usuarios. Hay varios componentes dentro 
de él, el administrador de nodos, el nodo y las aplicaciones 
que se ejecutan en la red en malla, como el cliente vpn, el 
servidor vpn, el cliente de socks, el servidor de socks, etc.

Todos los componentes dentro de Skywire se basan en la 
biblioteca de signal como un cliente de signal. Entonces, 
pueden ser ejecutados, administrados y apagados por medio de Viscript.

## Architecture

#### Architecture Diagram:

------

```
                                   +-----------+-------------+
           +---------------^-----+ |     vpn   |    socks    |
           |  managed by   |       +-----------+-------------+
           |               <-----+ |          node           |
           v               |       +-------------------------+
                           <-----+ |       node manager      |
+-------------------+      |       +-------------------------+
|      viscript     |      +-----+ |        messenger        |
+-------------------+--------------+-------------------------+
|                        signal                              |
+------------------------------------------------------------+
|                         net                                |
+------------------------------------------------------------+
```

------

There are client side and server side applications for each service, like vpn client and vpn server. They runs on Skywire meshnet. As we know, Skycoin is the currency of Skywire, when the user forwards traffic or provide network resources, he or she receives Skycoin. Likewise, when the user consumes network resources or media, he or she spends Skycoin. Once metering and settlement is implemented, Skywire will generate coins for operating the network.

Node, Node Manager and Messenger are the key components of Skywire meshnet. Node is a peer to peer mesh node. Service applications will register to Node, and their traffic will be forwarded by Node. Node Manager manages the routes between nodes in meshnet. Messenger allows users to peer clusters by public key. They are the cornerstones of Skywire meshnet.

## Summary

Viscript and Skywire are still under heavy developments. But we have achieved many exciting milestones around the skycoin ecosystem. And we are enjoying and going to unlock the full potential of a free internet in the future!

#### Sky-Messenger screenshot:

![screenshot](messenger.png)
