# Chapitre 9 — distribute et le partage des recompenses de staking

`RocketMegapoolDelegate.distribute` repartit les recompenses de staking accumulees par les validateurs d'un megapool entre plusieurs beneficiaires. `_distributeAmount` calcule quatre parts a partir du montant total (`calculateRewards`) : une part pour l'operateur de noeud (`nodeAmount`), une part pour les votants de la gouvernance (`voterAmount`, envoyee a `RocketRewardsPool`), une part pour le DAO du protocole (`protocolDAOAmount`), et une part pour les detenteurs de rETH (`rethAmount`, envoyee directement au contrat rETH pour faire croitre le taux de change du chapitre 4).

Si l'operateur a une dette envers le protocole (`debt`, par exemple issue d'une penalite ou d'une avance), sa part de recompenses sert d'abord a rembourser cette dette avant que le reliquat ne soit crediteé a `refundValue`, le solde que l'operateur peut effectivement reclamer via `claim`. Cette priorite de remboursement automatique garantit que le protocole recupere ses creances sans intervention manuelle, directement au fil des cycles de distribution.

La distribution est bloquee tant qu'un validateur du megapool est en cours de sortie (`numExitingValidators`) ou verrouille suite a une contestation (`numLockedValidators`) : ces etats representent des situations ou le montant exact des recompenses dues n'est pas encore stabilise, et distribuer prematurement risquerait de mal repartir des fonds entre les differentes parties prenantes.

[Chapitre suivant : RocketNetworkBalances, l'oracle de soldes reseau](10-network-balances.md)
