# Livrables

Ce dossier contient tous les outputs produits par Claude pour Bilal.

## Règle d'or

| Type | Destination |
|------|-------------|
| **Inputs** (documents que tu fournis : PDFs, exports, notes, briefs) | `context/import/` |
| **Outputs** (ce que Claude produit pour toi) | `livrables/` |

## Organisation

```
livrables/
├── sites-web/     Sites internet, landing pages, maquettes
├── applications/  Outils, scripts, automatisations
├── rédaction/     Briefs, articles, templates éditoriaux
└── cabinet/       Livrables pour Les Rois du Web (agence)
```

## Convention de nommage

Format : `AAAA-MM-JJ_nom-du-projet_type.ext`

Exemples :
- `2026-06-25_landing-couvreurs-idf_v1.html`
- `2026-06-25_script-devis-vocal_v2.py`
- `2026-06-25_brief-article-seo_template.md`

Règles :
- Toujours préfixer par la date au format `AAAA-MM-JJ`
- Minuscules, tirets, pas d'espaces ni d'accents dans le nom de fichier
- Indiquer la version (`_v1`, `_v2`) quand le fichier évolue
- Indiquer le type à la fin quand utile (`_brief`, `_script`, `_template`, `_final`)
