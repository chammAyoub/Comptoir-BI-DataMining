# Reprise de l’environnement — à valider

Ces indications expliquent les dépendances visibles dans les sources. Elles ne sont pas une procédure testée de réinstallation.

## Ce qui peut être consulté sans installation BI

- Captures de tableaux de bord, du modèle en étoile, des flux SSIS et des résultats de mining.
- Requête SELECT de transformation et fichiers XML des projets.
- Échantillons CSV/ARFF et structure du workflow Orange.

## Pour relancer SSIS / SSAS ultérieurement

1. Disposer d’un environnement Windows avec SQL Server et Analysis Services, Visual Studio et les extensions SSIS/SSAS compatibles. Le projet SSIS cible SQL Server 2022 ; ses fichiers de solution sont au format `.slnx`, dont la prise en charge dépend de la version de Visual Studio. Il est aussi possible d’examiner les projets `.dtproj` et `.dwproj` directement.
2. Retrouver ou reconstruire les bases source et entrepôt à partir des schémas attendus. Aucun backup ni script complet de création/chargement n’est fourni. La requête `sql/fact_sales_source.sql` est un SELECT de transformation, pas un script de restauration.
3. Reconfigurer les connexions `YOUR_SQL_SERVER` des packages SSIS et de `ComptoirDW.ds`. Les sources utilisent le fournisseur historique SQL Native Client `SQLNCLI11.1` : vérifier sa disponibilité ou adapter les connexions et leurs métadonnées.
4. Vérifier les mappings, les types et les clés. Charger les dimensions Client, Produit, Temps avant les faits. La présence de composants Script peut nécessiter un environnement VSTA compatible.
5. Reconfigurer le serveur SSAS de déploiement et le mode d’authentification, puis vérifier le traitement des dimensions et du cube. Aucun déploiement n’a été réalisé pour préparer cette archive.

## Power BI

Le fichier original `power-bi/first_bi_project.pbix` est inclus et n’a pas été modifié. Son contenu a été inspecté statiquement : trois pages de rapport et une connexion `analysisServicesDatabaseLive` à `Projet_Cube_Comptoir` / `Cube_Ventes`. Il n’embarque pas de modèle de données `DataModel`.

L’ouvrir avec Power BI Desktop nécessite de disposer du cube SSAS attendu ou de reconfigurer la source vers un cube compatible. Le nom du serveur d’origine est conservé dans le binaire ; la chaîne de connexion ne contient pas de champ identifiant ou mot de passe. Ce fichier n’a pas été ouvert dans Power BI Desktop pour préparer la publication.

## Weka et Orange

- Weka : consulter `Clients_Retard.csv` et `Clients_Retard.arff`. Le second comporte déjà une colonne de cluster. Pour refaire K-means, repartir des attributs numériques appropriés ; ne pas utiliser l’identifiant client ou une colonne de résultat comme variables d’apprentissage. Les paramètres historiques complets et le modèle ne sont pas disponibles.
- Orange : installer Orange Data Mining et l’extension `Orange3-Associate`, ouvrir `apriori.ows`, puis sélectionner `Panier_Produits.csv` dans le widget File. Les réglages de règles d’association existants ont été conservés ; seul l’historique local de choix de fichiers a été réinitialisé. Ce workflow n’a pas été exécuté pour la publication.

## Modifications de préparation

Les transformations ETL et la définition du cube ont été conservées. Les métadonnées de machine, les serveurs de connexion et les caches de mise en page SSIS ont été retirés ou neutralisés. Les sources restent historiques : aucune correction métier ou reconstruction fonctionnelle n’a été entreprise.
