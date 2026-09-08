# Chapitre 8 — RocketNodeStaking, le collateral RPL

Au-dela du bond en ETH, un operateur de noeud doit egalement staker le jeton natif de gouvernance du protocole, RPL, comme garantie supplementaire. `RocketNodeStaking.sol` gere ce cycle : `stakeRPL`/`stakeRPLFor` transferent du RPL vers le contrat et augmentent le solde stake de l'operateur ; `unstakeRPL` ne retire pas immediatement le RPL mais le fait transiter par un etat "unstaking" pendant une periode de detention configurable (`getUnstakingPeriod`), avant que `withdrawRPL` ne le rende disponible — un delai qui empeche un operateur de retirer sa garantie juste avant d'etre sanctionne pour mauvaise conduite.

`stakeRPLFor`/`unstakeRPLFor`/`withdrawRPLFor` permettent a une adresse tierce d'agir pour le compte d'un operateur, sous reserve d'une autorisation explicite (`setStakeRPLForAllowed`, une liste blanche par appelant) ou d'etre l'adresse de retrait RPL de l'operateur elle-meme — un mecanisme utile pour des services tiers (par exemple des pools de RPL delegue) qui gerent le staking pour le compte de plusieurs operateurs sans en detenir directement les cles.

Le RPL stake sert de collateral economique qui aligne les interets de l'operateur avec le bon fonctionnement du reseau : en cas de faute (double signature, temps d'arret prolonge, soumission de donnees d'oracle incorrectes), une partie de ce RPL peut etre confisquee, une sanction bien plus severe et rapide a appliquer qu'une perte de mise en ETH bloquee sur la Beacon Chain.

[Chapitre suivant : distribute et le partage des recompenses](09-distribution.md)
