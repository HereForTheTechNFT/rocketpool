# Chapitre 2 — Architecture du depot : stockage central et contrats modulaires

Rocket Pool est compose de dizaines de petits contrats specialises (`contracts/contract/`), organises par domaine : `deposit/` pour l'entree des fonds, `token/` pour rETH et RPL, `node/` pour la gestion des operateurs et leur collateral RPL, `megapool/` (et `minipool/`, l'ancienne generation) pour les conteneurs de validateurs, `network/` pour les oracles de prix et de soldes, `dao/` pour la gouvernance, `rewards/` pour la distribution des recompenses.

Deux contrats jouent un role transversal. `RocketStorage.sol` est l'unique base de donnees persistante du protocole : elle stocke des valeurs typees (chaines, entiers, adresses, booleens...) dans des mappings generiques indexes par un hash `keccak256`, jamais directement dans l'etat de chaque contrat metier. `RocketBase.sol` est le contrat abstrait dont herite chaque contrat metier ; il ne stocke rien lui-meme mais fournit des fonctions d'acces a `RocketStorage` (`getUint`, `setBool`, `getAddress`, etc.) ainsi qu'une serie de modificateurs de controle d'acces (`onlyRegisteredNode`, `onlyTrustedNode`, `onlyRegisteredMinipoolOrMegapool`, `onlyGuardian`).

Cette separation entre logique (dans des contrats remplacables) et donnees (dans un seul `RocketStorage` permanent) est le coeur du mecanisme de mise a niveau du protocole, detaille au chapitre suivant : un contrat metier peut etre entierement redeploye avec un nouveau code sans perdre ni migrer aucune donnee, puisque les donnees n'ont jamais vecu dans ce contrat.

[Chapitre suivant : RocketStorage et le registre de contrats upgradables](03-rocketstorage.md)
