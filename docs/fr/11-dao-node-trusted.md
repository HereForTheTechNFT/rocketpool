# Chapitre 11 — RocketDAONodeTrusted, le DAO des noeuds de confiance (Oracle DAO)

`RocketDAONodeTrusted.sol` gouverne l'ensemble restreint de noeuds autorises a soumettre des donnees d'oracle (prix RPL, soldes reseau du chapitre 10, penalites) — communement appele l'"Oracle DAO", distinct du DAO protocolaire plus large ouvert a tous les detenteurs de RPL stake.

Le contrat demarre en "mode bootstrap" (`onlyBootstrapMode`), ou le guardian de deploiement peut ajouter des membres et configurer des parametres directement (`bootstrapMember`, `bootstrapSettingUint`, `bootstrapSettingBool`, `bootstrapUpgrade`) le temps que le reseau amorce sa gouvernance decentralisee. `bootstrapDisable`, avec sa confirmation explicite (`_confirmDisableBootstrapMode`), desactive definitivement ce mode : au-dela, toute evolution des membres ou des parametres passe par des propositions et un vote des membres existants (`RocketDAONodeTrustedProposals`, un contrat separe).

`memberJoinRequired` prevoit un mecanisme de secours : si le nombre de membres tombe sous un minimum (`daoMemberMinCount`, fixe a trois), le DAO entre en "mode bas effectif" (`onlyLowMemberMode`) ou un nouveau noeud peut rejoindre sans passer par un vote complet — une protection contre le risque qu'un DAO trop reduit devienne incapable de reunir le quorum necessaire pour se reparer lui-meme.

[Chapitre suivant : RocketVault, la tresorerie centrale](12-vault.md)
