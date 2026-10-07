<p align="center">
  <img src="assets/header.svg" alt="En-tête : Audit migration CRM" width="100%">
</p>

# Audit qualité CRM avant migration Salesforce → HubSpot

Notebook pandas / matplotlib qui audite la qualité de données CRM synthétiques (734 comptes, 5 234 contacts) comme préparation d'une migration Salesforce → HubSpot : mesure des anomalies, nettoyage, analyse commerciale, cartographie des objets et recommandations. Aucune migration réelle n'a été réalisée.

## Contexte

Lors d'une migration CRM, un risque important est la qualité des données : champs manquants, formats incohérents et doublons compromettent la fiabilité du pipeline commercial et du reporting une fois les données transférées. Ce projet reproduit la démarche d'un analyste RevOps qui prépare une telle migration : auditer avant l'import, quantifier les anomalies, puis formuler des recommandations pour sécuriser la migration.

Limites de périmètre : les données sont synthétiques et ne proviennent d'aucune entreprise ; je n'ai pas de pratique de CRM en entreprise ; Salesforce relève de l'apprentissage ; ma pratique de HubSpot se limite à un compte gratuit et n'est pas documentée dans ce dépôt.

## Données

Jeu public et **synthétique** « Synthetic B2B CRM & Marketing Dataset » (Kaggle), en deux entités :

- **Companies :** 734 comptes, 19 champs ;
- **Employees :** 5 234 contacts, 21 champs.

Le jeu fournit une version « propre » et une version « bruitée » (anomalies volontaires). L'audit porte sur la version bruitée ; la version propre sert de référence. **Les fichiers CSV ne sont pas dans ce dépôt** (voir « Reproduire »).

## Démarche

Tout est dans `notebooks/audit_crm.ipynb` :

1. **Audit qualité** : valeurs manquantes, incohérences de casse, fautes de frappe dans `Industry`, score de complétude.
2. **Nettoyage et standardisation** : normalisation de la casse, correspondance des fautes de frappe d'`Industry`, imputation par la médiane (champs numériques) ou le mode (champs catégoriels).
3. **Analyse commerciale** : taux de conversion par secteur, performance par taille d'entreprise, réponse aux campagnes des décideurs, canaux de contact, pipeline de contrats.
4. **Cartographie Salesforce → HubSpot** : 6 objets (Account, Contact, Opportunity, Lead, Owner, Campaign) avec un niveau de risque par objet.
5. **Recommandations** : 5 actions priorisées (P1 à P3).

## Résultats et limites

Constats lus dans les sorties du notebook :

- **Valeurs manquantes :** 52 dans Companies et 709 dans Employees, soit un score de complétude de 99,6 % et 99,4 %. Le champ le plus touché côté contacts est `Influence_Score` (136 valeurs manquantes, 2,6 %).
- **Casse incohérente sur 9 champs catégoriels.** Exemple : `Contract_Status` s'écrit de 8 façons pour 3 valeurs réelles (`Active`, `Pending`, `Expired`) ; `Seniority_Level`, 12 écritures pour 5 valeurs ; `Campaign_Type`, 22 écritures pour 17 une fois la casse normalisée.
- **Fautes de frappe dans `Industry` :** 16 valeurs détectées, corrigées par une table de correspondance ; 15 secteurs distincts après normalisation.
- **Pipeline de contrats après nettoyage :** 528 actifs, 135 en attente, 71 expirés.
- **Décideurs :** taux moyen de réponse aux campagnes de 8,0 %, contre 7,1 % pour les non-décideurs.
- **Canal de contact préféré :** l'e-mail arrive en tête (42,9 % des contacts), devant le téléphone (34,6 %) et LinkedIn (14,7 %) ; ces parts sont calculées avant normalisation de la casse.
- **Cartographie :** le niveau de risque est jugé élevé pour Opportunity → Deal (étapes de pipeline à personnaliser), moyen pour Contact (format des e-mails) et Lead (cycle de vie à redéfinir), faible pour les trois autres objets.

Limites :

- Données synthétiques : ces constats décrivent le jeu, pas un CRM d'entreprise.
- **Le nettoyage est partiel :** à la fin de l'exécution enregistrée, le notebook affiche encore 63 valeurs manquantes (Companies) et 824 (Employees). Le score « 100 % après nettoyage » de sa dernière cellule n'est donc pas démontré.
- Les écarts de taux de réponse (8,0 % contre 7,1 %) sont décrits sans test statistique.
- Le notebook ne contient pas de contrôle de la version nettoyée par rapport à la version propre du jeu.

## Structure du dépôt

```
├── notebooks/
│   └── audit_crm.ipynb        Notebook d'analyse (audit, nettoyage, analyses)
├── outputs/
│   ├── RevOps_CRM_Migration_Analysis.pdf   Rapport
│   └── *.png                  Graphiques
└── README.md
```

Les graphiques de `outputs/` ne correspondent pas tous aux fichiers enregistrés par la version actuelle du notebook.

## Reproduire

Le notebook charge cinq fichiers CSV du jeu Kaggle, absents du dépôt : `companies_clean_734.csv`, `companies_noisy_734.csv`, `employees_clean_5234.csv`, `employees_noisy_5234.csv` et `employees_with_company_sample.csv`. Pour le rejouer :

1. télécharger le jeu sur Kaggle et placer ces cinq fichiers dans le dossier d'exécution du notebook (`notebooks/`) ;
2. installer les dépendances (aucun fichier `requirements` dans le dépôt) : `pip install pandas numpy matplotlib seaborn jupyter` ;
3. ouvrir `notebooks/audit_crm.ipynb` et exécuter les cellules dans l'ordre.

## Stack

Python (pandas, numpy, matplotlib, seaborn) et Jupyter Notebook.

## Autrice

**Kadidiatou Ibrahima Bagayoko**, étudiante en Bachelor en Intelligence Artificielle (grade Licence), ECE Paris, spécialisation Data & IA. [LinkedIn](https://linkedin.com/in/kadi-bagayoko) · kadibaga22@gmail.com
