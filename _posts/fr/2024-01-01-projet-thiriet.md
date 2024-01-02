---
layout: post
comments: true
title: Transformation Numérique chez Thiriet, étude de cas
type: post
lang: fr
tags: [GitOps, finops, terraform, entreprise scale, azure]
image: /images/post/2024/01/thiriet-migration-cloud.png
summary: "Retour d'experience sur le déploiement d'entreprise scale landing zone"
---

# Transformation Numérique chez Thiriet : Une Étude de Cas par un Architecte Cloud d'Orange Business Services

## Introduction

Dans le monde en constante évolution de la technologie cloud, la modernisation des systèmes d'information représente un enjeu crucial pour les entreprises souhaitant rester compétitives. Mon expérience récente en tant qu'architecte cloud computing chez Orange Business Services, spécialisés dans les technologies Microsoft Azure, offre un aperçu fascinant de cette transformation. Ce billet de blog se concentre sur le projet de migration et de modernisation du système de prise de commande par téléphone pour Thiriet, un défi à la fois complexe et enrichissant car il s'agit de moderniser une sinon la principale application métier du groupe. Cette application génère plusieurs centaines de millions d'euros de chiffre d'affaires et le nombre d'utilisateurs est très important. 

## Contexte du Projet

Thiriet, face à la nécessité de moderniser ses applications métiers et d'améliorer l'efficacité de ses opérations, s'est tourné vers Orange Business Services pour une solution cloud. Le projet comprenait la migration de six applications métiers, écrites dans divers langages comme Java, NodeJs, .Net et Crystal Report, vers un environnement Azure. Le projet, pour être soutenable devait s'appuyer sur une migration progressive de cet ensemble d'applications, en assurant une montée a bord progressive des équipes de développement applicatif et sysops du groupe. Les aspects financiers et réglementaires étaient bien évidemment très importants surtout dans le monde du retail ou la pression sur les marges est important et le contexte de hausse des coûts de l'énergie représentaient un défi majeur pour ce spécialiste de la chaine du froid.

## Défis et Solutions

Dès nos premiers échanges, le client a compris que le principal levier de réduction des coûts à long terme nécessitait une approche coordonnée de migration vers des scénarios d'hébergement PAAS. Azure offre une large panoplie de services qui ont pu être orchestrés pour rendre un service de qualité, performant et sécurisé. Les principaux enjeux ont été:

1. **Modernisation des Applications** : Le premier défi a été la containerisation des applications pour une exécution au sein d'app service qui offrent une première approche de gestion des containers sans la difficulté de gestion de Kubernetes hors de portée pour la DSI Thieriet. Cette étape a nécessité une révision des fonctionnalités clés de certaines applications telles que la gestion des sessions et le mécanisme d'injection de dépendance.

2. **Gestion des Impressions** : Un défi particulier était de maintenir la capacité à gérer 200 000 impressions papier par jour, tout en migrant vers le cloud. Constuire une API de gestion d'impression, sollicitable au travers de l'hybridation réseau a été la solution.

3. **Adoption du Processus CAF de Microsoft** : L'utilisation du Cloud Adoption Framework (CAF) a été essentielle pour assurer une transition réussie, tant sur le plan technique que financier. Sans cette approche, le cadrage projet aurait été difficile avec une dispersion de l'énergie tant dans les phases de design que de déploiement. 

4. **Stratégie Multi-Tenancy** : Nous avons revu la stratégie multi-tenancy pour permettre la mutualisation des instances applicatives à travers 82 centres d'appels.

5. **Synchronisation des Données** : La synchronisation en temps réel des données métiers depuis le cloud Azure vers les systèmes internes a été un autre aspect crucial du projet. La synchronisation des données SQL en temps réels ainsi que la gestion des fichiers de commande (> 10000/ jours) a représenté le principal challenge.

## Résultats et Avantages

La migration vers Azure a apporté plusieurs avantages significatifs :

1. **Gain de Productivité** : grâce à l'automatisation des déploiements et des tâches administratives.

2. **Efficacité financière** : réduction notable du coût total de possession.

3. **Amélioration de la Disponibilité** : réduction drastique des incidents et orientation des demandes de support vers des évolutions plutôt que des corrections.

4. **Transformation méthodologique** : adoption de pratiques DevSecOps et d'agilité par les équipes de développement et d'infrastructure, soutenue par l'implémentation de la chaine d'outillage Azure DevOps.

## Conclusion

Le projet de modernisation de son infrastructure aura chez Thiriet amélioré l'infrastructure technique, mais a également entraîné un changement culturel significatif. Cette expérience souligne l'importance d'une planification stratégique et d'une gestion efficace pour réussir dans la migration cloud. Pour les entreprises envisageant une transformation similaire, il est crucial de s'appuyer sur un partenaire compétent, de privilégier une approche de modernisation et de s'engager pleinement dans le processus tant au niveau technique que managérial. 

Merci aux équipes Thiriet de m'avoir fait confiance pour cette réalisation qui aura été pour moi une de mes meilleurs réalisation 2023.