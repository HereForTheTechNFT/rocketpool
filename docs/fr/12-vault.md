# Chapitre 12 — RocketVault, la tresorerie centrale du protocole

`RocketVault.sol` centralise la garde de tous les fonds du protocole (ETH et jetons ERC-20) au nom des differents contrats metier, plutot que de laisser chaque contrat (pool de depot, megapools, DAO de recompenses...) detenir directement son propre solde. `balanceOf`/`balanceOfToken` exposent, pour un nom de contrat logique donne, le solde qui lui est attribue en interne.

`depositEther`/`withdrawEther` et leurs equivalents pour les jetons (`depositToken`, `withdrawToken`, `transferToken`, `burnToken`) sont tous restreints par `onlyLatestNetworkContract` : seul un contrat officiellement enregistre dans `RocketStorage` (chapitre 3) peut deplacer les fonds qu'il detient dans le coffre. C'est le meme motif que la comptabilite interne du Vat de MakerDAO ou le stockage des jetons du Vat d'Uniswap v3 : la tresorerie effective (ici de veritables jetons ERC-20 et de l'ETH, contrairement au Vat qui ne stocke que des soldes abstraits) est separee de la logique metier, ce qui reduit la surface d'attaque de chaque contrat individuel — un bug dans un contrat metier ne peut a lui seul deplacer que les fonds explicitement attribues a ce contrat, jamais l'intégralite de la tresorerie du protocole.

Cette centralisation facilite egalement les mises a niveau (chapitre 3) : quand un contrat metier est remplace, ses fonds restent en securite dans `RocketVault` et sont simplement re-attribues au nouveau contrat enregistre sous le meme nom logique, sans transfert physique de fonds necessaire.

[Chapitre suivant : limites et perimetre](13-limites-et-perimetre.md)
