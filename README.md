# Tuto maker

Outil web **100 % hors ligne** pour créer des marches à suivre techniques illustrées et les exporter en PDF. Un seul fichier HTML, sans installation ni serveur.

Conçu pour un contexte laboratoire / mécanique de précision, mais utilisable pour n'importe quelle procédure pas-à-pas.

## Fonctionnalités

- **Étapes ordonnables** — ajout, suppression, réordonnancement par glisser-déposer ou flèches.
- **Images par étape** — ajout par clic, collage (`Ctrl+V`) ou glisser-déposer. Compression automatique à l'import pour garder les fichiers légers.
- **Annotations vectorielles** — flèche, cercle/ellipse, rectangle et texte libre, en plusieurs couleurs et épaisseurs. Les annotations restent modifiables (stockées séparément de l'image, jamais aplaties dans le projet).
- **Métadonnées** — titre, code article, auteur, service/atelier et description/objectif.
- **Export PDF** soigné — en-tête et pied de page sur chaque page, page de titre, grille d'images, tableau d'historique des versions. Format A4 portrait.
- **Aperçu PDF (brouillon)** — génère un PDF de contrôle **sans incrémenter la version** : utile pour vérifier le rendu avant de figer une version officielle.
- **Historique des versions** — éditable, incrémenté automatiquement à chaque export officiel.
- **Sauvegarde / chargement `.json`** — le projet complet (texte, images, annotations, versions) tient dans un seul fichier portable.

## Utilisation

1. Ouvrir `tuto-maker.html` dans un navigateur récent (rien à installer).
2. Renseigner les **informations générales** (titre, code article, auteur, atelier…).
3. Cliquer sur **Ajouter une étape**, saisir le titre et la description.
4. Ajouter des images à une étape (clic sur la zone, `Ctrl+V`, ou glisser-déposer).
5. Cliquer sur **✎** sur une image pour l'annoter, puis **Valider les annotations**.
6. **Sauvegarder .json** pour conserver le projet (et le rouvrir plus tard via **Ouvrir .json**).
7. **Aperçu PDF** pour un brouillon de contrôle, ou **Exporter PDF** pour produire la version officielle.

## Export PDF : officiel vs brouillon

| | Exporter PDF | Aperçu PDF |
|---|---|---|
| Incrémente la version | Oui | Non |
| Ajoute une ligne à l'historique | Oui | Non |
| Vide les notes de version | Oui | Non |
| Marquage | — | Tag `BROUILLON` + filigrane |
| Nom du fichier | `..._v{n}.pdf` | `..._v{n+1}_BROUILLON.pdf` |

Le brouillon affiche le rendu de la **prochaine** version officielle sans la déclencher.

## Format de sauvegarde

Le projet est sérialisé en JSON :

- `meta` — titre, code article, auteur, service, description.
- `steps[]` — chaque étape contient `title`, `text` et `images[]` (image en data-URL + `annotations[]` vectorielles).
- `version` — numéro courant, notes du prochain export et historique.

Les images étant intégrées au JSON (en base64, après compression), le fichier est autonome : aucun chemin externe à gérer.

## Détails techniques

- **Fichier HTML unique**, sans étape de build et sans dépendance réseau.
- **pdf-lib v1.17.1** (MIT) inliné pour une génération PDF entièrement hors ligne.
- Compression d'image à l'import : largeur max 1600 px, ré-encodage JPEG (qualité ~0.85) ; les PNG sont conservés pour préserver la transparence.
- **Aucune donnée n'est envoyée** : tout reste dans le navigateur. Confidentialité totale, fonctionne sur poste isolé.

## Hébergement

Servi en statique sur GitHub Pages (`kiliangir.github.io`). Il suffit d'ouvrir le fichier HTML, en local comme en ligne.

## Convention de commits

Les messages de commit suivent la convention *Conventional Commits*, rédigés en français (ex. `feat(pdf): ajouter l'export en mode brouillon`).
