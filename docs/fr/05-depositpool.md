# Chapitre 5 — RocketDepositPool et les files d'attente de validateurs

`RocketDepositPool.sol` est le point d'entree unique des depots utilisateurs : `deposit()` verifie que le montant respecte les bornes configurees et que la capacite totale (solde du pool plus capacite effective de la file d'attente de validateurs) n'est pas depassee, avant de faire frapper le rETH correspondant par `RocketTokenRETH`.

Le pool distingue la part "utilisateur" (les depots en attente d'etre assignes a des validateurs) de la part "operateur" (`getNodeBalance`, l'ETH que des operateurs de noeud ont deja avance dans le pool en attendant que leur validateur soit finance) : `getUserBalance` peut meme etre negatif si les operateurs ont temporairement avance plus que ce que les deposants ont fourni.

Deux files d'attente coexistent, `expressQueueNamespace` et `standardQueueNamespace` : un operateur peut payer pour utiliser un "ticket express" (`_useExpressTicket` dans `RocketMegapoolDelegate.newValidator`, chapitre 6) afin que son validateur soit finance en priorite des que des fonds sont disponibles, plutot que d'attendre son tour dans la file standard — un mecanisme de marche pour prioriser l'allocation de capital limite entre operateurs en concurrence.

`getExcessBalance` calcule l'ETH du pool qui n'est engage envers aucun validateur en attente : c'est cette portion excedentaire qui alimente la liquidite immediate de retrait de rETH (chapitre 4), tandis que le reste reste reserve au financement des validateurs deja dans la file.

[Chapitre suivant : le megapool, conteneur de validateurs par operateur](06-megapool.md)
