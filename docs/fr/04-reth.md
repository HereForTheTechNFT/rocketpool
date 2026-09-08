# Chapitre 4 — RocketTokenRETH, le jeton de liquid staking et son taux de change

`RocketTokenRETH.sol` est le jeton ERC-20 remis aux deposants. A la difference du stETH de Lido (rebasement par variation du solde), rETH suit le modele du "taux de change croissant", le meme principe que l'aToken d'Aave ou le vault ERC-4626 : le nombre de jetons rETH d'un utilisateur reste fixe apres son depot, mais chaque rETH vaut progressivement plus d'ETH avec le temps, a mesure que les recompenses de staking s'accumulent dans le systeme.

`getEthValue` et `getRethValue` sont les deux conversions centrales, toutes deux calculees a partir de `RocketNetworkBalances` (chapitre 10) : `ethValue = rethAmount * totalEthBalance / rethSupply`. `getExchangeRate` n'est qu'un appel de commodite a `getEthValue(1 ether)`. `mint`, appelable uniquement par `RocketDepositPool`, frappe le montant de rETH correspondant a l'ETH deposeee au taux courant. `burn` fait l'inverse cote utilisateur : elle detruit le rETH et transfere l'ETH equivalent, en allant chercher au besoin de la liquidite supplementaire aupres du pool de depot via `withdrawDepositCollateral`.

`_beforeTokenTransfer` impose un delai minimal entre un depot et tout transfert ou retrait ulterieur du rETH obtenu (`network.reth.deposit.delay`), une protection contre certaines attaques de type sandwich autour de la mise a jour du taux de change. `depositExcessCollateral` renvoie automatiquement vers le pool de depot l'ETH detenu par le contrat rETH au-dela d'un taux de collateral cible, pour que la liquidite disponible aux retraits immediats reste maitrisee sans immobiliser plus d'ETH que necessaire.

[Chapitre suivant : RocketDepositPool et les files d'attente de validateurs](05-depositpool.md)
