# Projet 2 INF1007 : popularité des commandants Magic (Scryfall + otags)

> **Statut : volet 1 (jeu de données).** Les volets 2 (modèle) et 3 (interface) restent à faire.

## Description

Prédire la **position** (1 = le plus populaire) d'un commandant de Magic: The Gathering
parmi les 1000 commandants les plus populaires, à partir de ses *otags* (Oracle Tags de
Scryfall Tagger : `ramp`, `removal`, `draw`, etc.) et de quelques caractéristiques de la carte.
Problème de **régression**.

## Jeu de données

- **Source** : API Scryfall (https://scryfall.com/docs/api). L'ordre de popularité vient de
  `order:edhrec` (données EDHREC reprises par Scryfall). Les otags viennent de Scryfall Tagger
  (tags de la communauté), interrogés via le filtre de recherche `otag:`.
- **Date de collecte** : _à compléter_
- **Licence / conditions d'utilisation** : _à compléter après lecture de https://scryfall.com/docs/api
  (le CSV est-il redistribuable dans un dépôt public ?)_
- **Colonnes** : `nom`, `position` (cible), `cmc`, `annee`, `identite_couleur`,
  `type_principal`, `nb_tags`, et une colonne `tag_<nom>` (0/1) par otag.
- Aucune donnée personnelle.

## Installation et lancement

```bash
python -m venv .venv
source .venv/bin/activate        # Windows : .venv\Scripts\activate
pip install -r requirements.txt

python -m src.main collecter     # télécharge, construit donnees/commandants_top1000.csv
python -m src.main verifier      # vérifie les critères de la section 2
pytest                           # tests (hors ligne)
coverage run -m pytest && coverage report -m
```

La collecte fait environ 150 à 400 requêtes (quelques minutes) ; les résultats sont mis en
cache dans `donnees/cache_*.json` (ignorés par Git). Un tag qui ne renvoie aucun résultat est
signalé : corrigez-le dans `TAGS_PAR_DEFAUT` (`src/donnees.py`) ou avec `--tags a,b,c`.

## Structure

```
src/exceptions.py   exceptions personnalisées (DonneeInvalideError, ScryfallError)
src/donnees.py      appels API, cache JSON, construction du DataFrame, vérification des critères
src/main.py         point d'entrée (ligne de commande)
tests/              tests pytest (API simulée, aucun accès réseau)
donnees/            CSV et graphique d'exploration
modeles/            modèle sauvegardé + métadonnées JSON (volet 2)
```

## Outils du chapitre 7

- **Décorateur** `@chronometre` : affiche la durée des étapes de collecte.
- **Générateur** `lire_csv_par_blocs` : lit le CSV par blocs de lignes.

## Couverture de tests

_À compléter avec le résultat de `coverage report`._

## Documentation consultée

- pandas : _à compléter_
- scikit-learn : _à compléter_
- Technologie d'interface : _à compléter_
- API Scryfall : https://scryfall.com/docs/api
