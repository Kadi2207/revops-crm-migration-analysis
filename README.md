# CRM Migration & RevOps Analysis — Salesforce → HubSpot

Audit qualité et analyse de préparation à une migration CRM, dans une logique RevOps.

## Contexte

Lors d'une migration CRM de Salesforce vers HubSpot, le principal risque n'est pas technique
mais lié à la **qualité des données** : champs manquants, formats incohérents et doublons
compromettent la fiabilité du pipeline commercial et du reporting une fois les données transférées.

Ce projet reproduit la démarche d'un analyste RevOps préparant une telle migration : auditer les
données avant l'import, quantifier les anomalies, puis formuler des recommandations concrètes pour
sécuriser la migration.

## Données

Le projet s'appuie sur un jeu de données B2B CRM public et **synthétique**
(Synthetic B2B CRM & Marketing Dataset — Kaggle), composé de deux entités
reflétant la structure d'un CRM réel :

- **Companies** (734 comptes) — entreprises clientes
- **Employees** (5 234 contacts) — personnes rattachées à ces entreprises

Le dataset fournit une version « propre » et une version « bruitée » (avec anomalies volontaires),
ce qui permet de démontrer un audit qualité réaliste et de vérifier les corrections apportées.

> Les données sont synthétiques : elles ne proviennent d'aucune entreprise réelle. Elles ont été
> choisies parce que leur structure et leurs anomalies reproduisent fidèlement celles d'un CRM B2B.

## Démarche

1. **Audit qualité** — mesure des valeurs manquantes, incohérences de format et doublons (sans modification des données)
2. **Nettoyage & standardisation** — normalisation des champs, correction des typos, imputation des valeurs manquantes
3. **Analyse business** — segmentation, pipeline contractuel, profils de contacts, canaux de communication
4. **Cartographie Salesforce → HubSpot** — correspondance des objets et champs, identification des risques
5. **Recommandations RevOps** — règles de validation, champs obligatoires, lead scoring

## Principaux constats

- Un même statut de contrat écrit de 8 façons différentes → segmentation faussée si importé tel quel
- Champ « secteur » avec plus de 30 variantes pour une quinzaine de valeurs réelles
- Les grandes entreprises génèrent en moyenne 24× plus de revenu que les PME → priorisation du pipeline
- Les contacts décideurs présentent un score d'influence supérieur de 32 % → justifie un lead scoring
- L'email domine en volume et en réactivité → canal prioritaire à structurer dans HubSpot

## Contenu du dépôt

```
├── notebooks/
│   └── audit_crm.ipynb        Notebook d'analyse (audit, nettoyage, visualisations)
├── outputs/
│   ├── RevOps_CRM_Migration_Analysis.pdf   Rapport complet
│   └── *.png                  Graphiques générés
└── README.md
```

## Stack technique

- **Python** (pandas, matplotlib) — audit, nettoyage et visualisation
- **Jupyter Notebook** — support de démonstration de la démarche

## Auteur

**Kadidiatou I. Bagayoko** — ECE Paris, Data & IA
kadibaga22@gmail.com
