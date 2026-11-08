# Deviens Français·e — lot 1 : socle sûr et réversible (v44 → v45)

Cette archive contient la livraison du lot 1 : les fichiers du site corrigés, les tests, le registre de contenu et la documentation. **Rien n'a été déployé sur OVHcloud** et **aucun commit Git n'a été fait** : ce sont des fichiers prêts à envoyer et à vérifier.

## Par où commencer

1. **`docs/hypotheses-et-decisions.md`** — ce que j'ai supposé, **les huit décisions que vous avez prises le 5 novembre 2026** avec ce qui en a été fait, et ce qui reste ouvert. À lire en premier.
2. **`docs/rapport-de-tests.md`** — pour chaque critère d'acceptation : exécuté ou non, commande, résultat réel. **Le critère 6 (OVH) n'est pas exécuté** : c'est à vous de le faire.
3. **`docs/deploiement-ovh.md`** — ce que vous faites chez OVHcloud (fichiers à envoyer dans l'ordre, commandes `curl` à lancer avant et après, résultats attendus) et ce que fait le code.
4. **`docs/retour-arriere.md`** — comment revenir à la v44 sans perdre la progression de personne.
5. `docs/journal-des-modifications.md` — chaque modification : emplacement, objectif, dépendances, comment la tester.

## Les fichiers à mettre en ligne : le dossier `site/`

`index.html`, `sw.js`, `confidentialite.html`, `.htaccess` (fichier caché), `manifest.webmanifest` et `icons/` (vos fichiers, identiques), `fonts/` (nouveau : deux polices et leurs licences). Empreintes SHA-256 : `docs/SHA256SUMS.txt`. Ordre d'envoi : `docs/deploiement-ovh.md`, § 3.

## Arborescence

```
.
├── LISEZ-MOI.md                      ce fichier
├── VERSION                           le numéro de version (45), écrit à un seul endroit
├── package.json · package-lock.json  outils de test : Playwright 1.63.0, axe-core 4.10.2
├── .gitignore · .gitattributes       pour créer un dépôt Git privé (docs/depot-git-prive.md)
│
├── site/                             ▶ LA LIVRAISON (à envoyer sur l'hébergement)
│   ├── index.html · sw.js · confidentialite.html · .htaccess      modifiés
│   ├── manifest.webmanifest · icons/                              vos fichiers, inchangés
│   └── fonts/                                                     nouveau (T4)
│
├── reference/                        les fichiers reçus le 2026-11-04 (v44), jamais modifiés
│   ├── index.html · sw.js · confidentialite.html · .htaccess
│   ├── SHA256SUMS-reference.txt
│   └── provided-by-owner/            vos exports, manifeste, icônes et le prompt (contiennent un prénom : dépôt privé)
│
├── docs/                             documentation (voir « Par où commencer »)
│   ├── hypotheses-et-decisions.md · changements-non-appliques.md · rapport-de-tests.md
│   ├── journal-des-modifications.md · journal-modifications-index.txt
│   ├── deploiement-ovh.md · retour-arriere.md · depot-git-prive.md
│   ├── index.html.diff · autres-fichiers.diff · SHA256SUMS.txt
│   └── outils/restaurer-copie-de-secours.js      restauration manuelle de la copie de secours locale
│
├── tests/                            17 tests + lanceur (tests/LISEZ-MOI.md)
│   ├── run-all.mjs                   une commande pour tout lancer : npm test
│   ├── *.test.mjs · lib/             un script par test, infrastructure commune
│   └── results/                      dernier rapport : rapport.md (lisible) et rapport.json
│
├── content/                          registre de contenu (content/LISEZ-MOI.md)
│   ├── registre-contenu.csv          315 questions + 4 constats hors lot
│   └── registre-annotations.json     sources, dates, statuts saisis à la main
│
├── scripts/
│   ├── appliquer-correctifs.mjs      produit site/ à partir de reference/ (78 remplacements : 74 dans index.html, 4 dans confidentialite.html ; chacun vérifié « exactement une fois »)
│   ├── correctifs/                   les morceaux de code insérés
│   ├── set-version.mjs               change le numéro de version partout (T18) : node scripts/set-version.mjs 46
│   ├── build-registre.mjs            produit le registre CSV
│   └── empreintes.mjs                calcule les empreintes SHA-256
│
└── research/                         notes de recherche (OVHcloud ; sources officielles pour le contenu ; correspondances Wikipédia ;
                                          R5 : comportement du navigateur pour les mises à jour du service worker, avec son script)
```

## Commandes utiles (depuis la racine de l'archive)

```
npm install && npx playwright install chromium    # une fois
npm test                                          # tous les tests, référence + livraison (4 min 11 s chez moi), rapport lisible
node scripts/appliquer-correctifs.mjs             # régénère site/ à partir de reference/ (résultat identique octet pour octet)
node scripts/set-version.mjs 46                   # prochaine version : un seul numéro à changer
sha256sum -c docs/SHA256SUMS.txt                  # vérifie les empreintes (Linux/macOS ; PowerShell : Get-FileHash)
```

## État du lot en une phrase

Les corrections du lot 1 sont écrites et vérifiées par 17 tests exécutés localement (verdict vert au dernier passage complet ; un échec isolé d'un test de mise à jour du service worker, corrigé côté tests, est expliqué au § 9 de `docs/rapport-de-tests.md`) ; **il reste à vous de valider le critère 6 sur OVHcloud** (redirection et en-têtes). Vos décisions du 5 novembre 2026 sont appliquées (`docs/hypotheses-et-decisions.md`, partie 3) ; les points encore ouverts y sont listés. Le lot 2 (code maintenable, CSP stricte) ne devrait commencer qu'ensuite.
