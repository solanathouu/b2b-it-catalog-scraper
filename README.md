# Scraper B2B - Catalogue IT TD Synnex

Outil d'extraction automatisée de données produits depuis le catalogue B2B **TD Synnex France (InTouch)**, couvrant 11 catégories de matériel informatique professionnel. Les données collectées alimentent un [configurateur B2B IT](https://v0.app/chat/remix-of-b2-b-it-configurator-2GPYXt2XzMs?ref=E8DTA3) permettant la recherche et la comparaison de produits.

## Contexte

TD Synnex est l'un des plus grands distributeurs mondiaux de solutions IT. Leur plateforme InTouch expose une API interne de recherche produits. Ce scraper exploite cette API pour extraire un catalogue complet de **+8 800 produits** avec leurs spécifications techniques détaillées (440+ attributs).

## Fonctionnalités

- Extraction automatisée de **11 catégories** de produits IT
- Pagination intelligente avec gestion des lots de 100 produits
- Récupération des prix via endpoint dédié (batch processing)
- Extraction dynamique de **440+ spécifications techniques** par produit
- Export CSV structuré prêt pour l'analyse ou l'intégration
- Rate limiting intégré (1 req/s) pour respecter le serveur

## Catégories extraites

| Catégorie | Description |
|-----------|-------------|
| PC All-in-One | Ordinateurs tout-en-un |
| Claviers | Périphériques de saisie |
| Moniteurs | Écrans et affichages |
| SSD Externes | Stockage externe SSD |
| Multifonctions | Imprimantes multifonctions |
| Mémoires | RAM et modules mémoire |
| Notebooks | Ordinateurs portables |
| Notebooks WS | Stations de travail portables |
| Hub/Switch | Équipements réseau |
| Téléphonie Mobile | Appareils mobiles |
| Ordinateurs de Bureau | Postes de travail fixes |

## Architecture technique

```
TD Synnex InTouch API
        │
        ├── /v1/public/fullsearch   ← Recherche produits (paginée)
        │
        └── /v3/public/price        ← Prix par lot d'IDs
        │
        ▼
   SCRAPPER_FINAL.py
        │
        ├── Authentification (cookie session)
        ├── Boucle sur 11 catégories
        ├── Pagination automatique (100 produits/page)
        ├── Récupération des prix par batch
        └── Extraction dynamique des spécifications
        │
        ▼
   produits_toutes_categories.csv
        │
        ▼
   Configurateur B2B IT (v0)
```

## Structure du projet

```
├── SCRAPPER_FINAL.py                # Script principal (production)
├── scrap_v1.py                      # Version de développement (mono-catégorie)
├── produits_toutes_categories.csv   # Données extraites (~8 800 produits, 11 MB)
└── README.md
```

## Prérequis

- **Python 3.x** (aucune dépendance externe, uniquement la bibliothèque standard)
- Un compte **TD Synnex InTouch** avec accès au catalogue France
- Connexion internet

## Installation et utilisation

### 1. Cloner le dépôt

```bash
git clone https://github.com/solanathouu/b2b-it-catalog-scraper.git
cd projet-scrapping
```

### 2. Récupérer le cookie d'authentification

1. Se connecter sur [TD Synnex InTouch](https://fr.tdsynnex.com/InTouch/MVC/ProductSearchPublicV2/Search?pageSize=20&page=1&sort=type%3DSTOCK&searchType=Category&refinements=eyJncm91cCI6IkNhdGVnb3JpZXMiLCJyZWZpbmVtZW50cyI6W3siaWQiOiJGQ1MiLCJ2YWx1ZUlkIjoiRkNTX1NVQkNMQVNTX1BDQUxMSU4xX0NPTURFU0sifV19)
2. Ouvrir les **DevTools** (F12) > onglet **Network**
3. Filtrer par `fullsearch` > cliquer sur la requête > copier la valeur du header **Cookie**

### 3. Configurer et lancer

```bash
# Remplacer le cookie ligne 8 de SCRAPPER_FINAL.py
# cookie = "VOTRE_COOKIE_ICI"

python SCRAPPER_FINAL.py
```

Le scraping prend environ **5 minutes** pour l'ensemble des 11 catégories.

## Données en sortie

Le fichier `produits_toutes_categories.csv` contient :

| Colonne | Description |
|---------|-------------|
| `id` | Identifiant unique du produit |
| `nom` | Nom commercial du produit |
| `marque` | Fabricant (HP, Dell, Lenovo, etc.) |
| `caracteristiques` | Description textuelle |
| `prix` | Prix catalogue (EUR) |
| `categorie` | Catégorie de rattachement |
| *+440 colonnes* | Spécifications techniques (processeur, RAM, stockage, résolution, connectique, etc.) |

## Stack technique

| Composant | Technologie |
|-----------|-------------|
| Langage | Python 3 (bibliothèque standard uniquement) |
| HTTP | `urllib.request` |
| Parsing | `json`, `re` |
| Export | `csv` |
| API cible | TD Synnex InTouch REST API |

## Lien avec le configurateur B2B

Les données extraites par ce scraper alimentent un **configurateur IT B2B** développé avec Next.js via [v0.dev](https://v0.app/chat/remix-of-b2-b-it-configurator-2GPYXt2XzMs?ref=E8DTA3). Ce configurateur permet de :
- Rechercher et filtrer des produits par catégorie et spécifications
- Comparer les caractéristiques techniques entre produits
- Consulter les prix du catalogue distributeur

## Avertissement

Ce projet a été réalisé dans un cadre éducatif et de démonstration technique. L'utilisation de ce scraper doit se faire dans le respect des conditions d'utilisation de TD Synnex et de la législation en vigueur. Le cookie d'authentification expire régulièrement et nécessite un renouvellement manuel.
