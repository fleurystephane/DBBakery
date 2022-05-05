# Infos Docker

## Création de l'image

Cette étape sera à faire à chaque changement de version de l'image ou plus exactement à chaque récupération sur un poste de développeur.

```
./postgres/build.sh
```

## Lancement du container

Avant toute chose il faut lancer l'image Docker Postgres :

```
./postgres/run.sh
```

On pourra stop le container si besoin (économie de ressources), les données seront conservées.

## Arrêt du container

Utilisation de Docker Desktop pour arrêter et supprimer un container.

# Liquibase

Pour le dév, on utilsera https://hub.liquibase.com/ avec le compte fleuryst et P..._72

## Génération du dbchangelog

Commande pour générer le fichier à partir de la base (idéal pour la création)
```
liquibase  --diffTypes=tables,functions,views,columns,indexes,foreignkeys,primarykeys,uniqueconstraints,data,storedprocedure,triggers,sequences --dataOutputDirectory=data --changeLogFile=dbchangelog.yaml generateChangeLog
```

Une fois les fichiers générées, il sera possible de renommer le csv afin de le conserver si besoin.

## Suppression des checksum 

Supprimer les checksums en période de dév uniquement au début du projet

```
liquibase clearCheckSums
```

## Mise à jour de la base suite changements sur changelog

Mettre à jour la base avec de nouvelles informations (nouveau changeset) :
```
liquibase update
```

Attention : les update suivent une logique de mise à jour par rapport à un historique. Donc si vous effacez l'historique, il faudra aussi supprimer toutes les tables dans la base sinon vous aurez un message indiquant que la table existe déjà...

## Mise à jour du changelog suite changements sur la base

Attention cette commande va overwriter le fichier changelog

```
liquibase generate-changelog --dataOutputDirectory=data --overwriteOutputFile=true
```
