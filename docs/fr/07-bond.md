# Chapitre 7 — Le bond de l'operateur et le calcul de la garantie requise

Le "bond" est la mise en ETH que l'operateur de noeud doit apporter pour chaque validateur qu'il fait tourner ; le reste, jusqu'a 32 ETH, provient du pool de depot alimente par les deposants de rETH. `RocketNodeDeposit.getBondRequirement`, consultee par `newValidator` et `reduceBond`, calcule ce montant en fonction du nombre de validateurs actifs de l'operateur — le protocole peut ainsi faire baisser l'exigence de mise par validateur a mesure qu'un operateur en possede davantage, une economie d'echelle qui abaisse la barriere a l'entree pour de nouveaux operateurs tout en recompensant ceux qui s'engagent durablement.

`getActiveValidatorCount` et `getNodeQueuedBond`/`getUserQueuedCapital` distinguent le capital deja assigne a des validateurs actifs de celui encore en file d'attente : `nodeBond` et `userCapital` suivent les montants effectivement stakes, `nodeQueuedBond` et `userQueuedCapital` ceux encore en attente d'assignation.

`reduceBond` permet a un operateur de reduire sa mise apres coup, si le nombre de validateurs actifs a grandi au point que l'exigence de bond par validateur a baisse : la difference est creditee a l'operateur via `applyCredit` (un solde utilisable pour financer un futur validateur sans nouvel apport d'ETH), sous reserve qu'aucun validateur ne soit encore en file d'attente ou en pre-stake — la reduction ne s'applique qu'a du capital deja pleinement actif, jamais a du capital en transit.

[Chapitre suivant : RocketNodeStaking, le collateral RPL](08-rpl.md)
