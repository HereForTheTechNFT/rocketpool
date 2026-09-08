# Chapitre 1 — Presentation de Rocket Pool

Rocket Pool est un protocole decentralise de liquid staking pour Ethereum. Il permet a deux profils d'utilisateurs tres differents de participer au staking : un deposant ordinaire peut deposer aussi peu que 0.01 ETH et recevoir en echange du rETH, un jeton liquide qui accumule automatiquement les recompenses de staking ; un operateur de noeud peut faire tourner un validateur avec seulement une fraction du capital normalement requis (32 ETH), le reste etant fourni par les deposants via le pool commun.

Cette double face du protocole est ce qui le distingue d'un simple pool de staking centralise : les operateurs de noeuds ne sont pas selectionnes ni approuves, n'importe qui peut en devenir un en apportant sa mise et son materiel, ce qui decentralise l'ensemble des validateurs Ethereum qui tournent grace au protocole plutot que de les concentrer chez un seul operateur.

Ce parcours s'appuie sur le depot cloné a la date d'ecriture, qui inclut deja la generation "megapool" (mise a niveau Saturn) : `contracts/contract/RocketStorage.sol`, `RocketBase.sol`, `RocketVault.sol`, `token/RocketTokenRETH.sol`, `deposit/RocketDepositPool.sol`, `megapool/RocketMegapoolDelegate.sol`, `node/RocketNodeStaking.sol`, `network/RocketNetworkBalances.sol`, `dao/node/RocketDAONodeTrusted.sol`. L'ancien systeme de "minipool" (un contrat par validateur) coexiste dans le depot avec le nouveau systeme de "megapool" (un contrat par operateur, portant plusieurs validateurs) ; ce parcours privilegie le megapool, la generation la plus recente.

Rien n'a ete installe, compile ni execute pour ecrire ces chapitres.

[Chapitre suivant : architecture du depot](02-architecture.md)
