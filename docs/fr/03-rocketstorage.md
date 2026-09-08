# Chapitre 3 — RocketStorage et le registre de contrats upgradables

`RocketStorage.sol` stocke, entre autres, un registre des contrats officiels du protocole : pour chaque nom logique (par exemple `"rocketDepositPool"`), une entree `contract.address` donne l'adresse actuellement active, et une entree booleenne `contract.exists` a cette adresse confirme qu'elle fait bien partie du reseau Rocket Pool courant. `RocketBase.getContractAddress` lit la premiere, le modificateur `onlyLatestNetworkContract` verifie la seconde avant d'accepter un appel entrant.

Ce registre permet de "mettre a niveau" un contrat metier sans toucher a `RocketStorage` : la gouvernance deploie une nouvelle version, l'enregistre sous le meme nom logique, et desenregistre l'ancienne adresse. Tous les autres contrats qui referencent ce nom logique (plutot qu'une adresse fixe) basculent alors automatiquement sur la nouvelle implementation au prochain appel — aucune migration de donnees n'est necessaire puisque `RocketStorage` n'a pas change d'adresse et continue de porter tout l'etat.

Un contrat guardian (`getGuardian`/`setGuardian`/`confirmGuardian`, un schema classique de transfert de propriete en deux etapes) controle ces operations sensibles pendant la phase de deploiement initial ; `storageInit`/`setDeployedStatus` marque le moment ou le controle bascule definitivement vers la gouvernance decentralisee du protocole (le DAO), le guardian perdant alors ses privilèges d'acces direct.

[Chapitre suivant : RocketTokenRETH, le jeton de liquid staking](04-reth.md)
