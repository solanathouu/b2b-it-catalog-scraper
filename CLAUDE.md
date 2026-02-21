# b2b-it-catalog-scraper

## Projet
Scraper B2B qui extrait ~8 800 produits IT depuis l'API TD Synnex InTouch (11 catégories), exporte en CSV, et alimente un configurateur B2B.

## Structure
```
SCRAPPER_FINAL.py    # Script principal (11 catégories, pagination, prix batch)
scrap_v1.py          # Version initiale (mono-catégorie, à des fins de démo/apprentissage)
produits_toutes_categories.csv  # Données extraites (~11 MB)
```

## Stack
- Python 3 (stdlib uniquement : urllib, json, csv, re, time)
- API REST TD Synnex InTouch (cookie session requis)

## Points d'attention
- Le cookie d'auth expire régulièrement → à renouveler manuellement
- `scrap_v1.py` contient un ancien cookie en clair (déjà dans l'historique git)
- Rate limiting : 1 req/s intégré dans le script

## Prochaine action
- Externaliser le cookie dans un fichier `.env` (+ python-dotenv) pour éviter les fuites
- Optionnel : ajouter un `.env.example` avec placeholder
