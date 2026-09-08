# Chapitre 6 — Le megapool, conteneur de validateurs par operateur

`RocketMegapoolDelegate.sol` est la generation la plus recente de la structure qui porte les validateurs d'un operateur de noeud (elle remplace le "minipool" historique, un contrat separe par validateur, par un seul contrat qui en porte plusieurs). Ce contrat sert lui-meme de credentials de retrait pour tous les validateurs Beacon Chain qu'il gere.

`newValidator` cree un nouveau validateur : elle verifie que le montant de mise apporte par l'operateur (`_bondAmount`) correspond exactement a l'exigence courante (chapitre 7), enregistre les donnees de depot (cle publique, signature, racine Merkle des donnees de depot, verifiees des cette etape pour garantir qu'un appel ulterieur au contrat de depot Beacon Chain ne pourra pas echouer), puis demande au pool de depot (`requestFunds`) la portion "utilisateur" du financement (32 ETH moins la mise de l'operateur).

`assignFunds`, appelee par le pool de depot une fois les fonds disponibles, effectue le "pre-stake" : un premier depot de 1 ETH sur le contrat de depot Beacon Chain (`casperDeposit.deposit`), suffisant pour ancrer les credentials de retrait sur la chaine sans engager tout le capital avant confirmation. `stake`, appelee ensuite par `RocketMegapoolManager`, effectue le depot des 31 ETH restants pour completer les 32 ETH necessaires a l'activation du validateur. Ce decoupage en deux depots (1 ETH puis 31 ETH) protege le protocole : un validateur mal configure ou dont l'operateur se retracte peut etre "dissous" (`dissolveValidator`) avant que la totalite du capital n'ait ete engagee sur la Beacon Chain.

`dequeue` permet a un operateur de retirer un validateur pas encore finance de la file d'attente, en recuperant sa mise sous forme de credit aupres du pool de depot plutot qu'en ETH immediat.

[Chapitre suivant : le bond de l'operateur et son calcul](07-bond.md)
