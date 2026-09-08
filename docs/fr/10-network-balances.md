# Chapitre 10 — RocketNetworkBalances, l'oracle de soldes reseau

Le taux de change rETH/ETH (chapitre 4) depend de la valeur totale d'ETH detenue par le reseau, y compris celle qui est verrouillee sur la Beacon Chain et donc invisible depuis un contrat Ethereum ordinaire. `RocketNetworkBalances.sol` est l'oracle qui comble ce manque : des noeuds de confiance (chapitre 11) observent l'etat de la Beacon Chain hors chaine et soumettent periodiquement, via `submitBalances`, le solde total du reseau, la portion actuellement en staking, et l'offre totale de rETH.

Chaque soumission est associee a un numero de bloc et un jeu de valeurs precis (`nodeSubmissionKey`, un hash de tous ces parametres) : un meme noeud ne peut soumettre qu'une fois pour un jeu de valeurs donne, et `submissionCount` compte combien de noeuds de confiance ont soumis exactement les memes valeurs. Une fois qu'un quorum est atteint (verifie dans la suite de la fonction, hors de l'extrait cite), les valeurs proposees deviennent les valeurs officielles du reseau, consultees par `getTotalETHBalance`, `getStakingETHBalance` et `getTotalRETHSupply`.

Ce mecanisme d'oracle par consensus de noeuds de confiance, plutot qu'un oracle de prix centralise unique, est ce qui permet a rETH de refleter fidelement l'etat reel du staking Ethereum (y compris les penalites subies par des validateurs) sans dependre d'un tiers de confiance unique susceptible de mentir ou d'etre corrompu.

[Chapitre suivant : RocketDAONodeTrusted, le DAO des noeuds de confiance](11-dao-node-trusted.md)
