---
layout: post
comments: true
title: Réussite de la Migration vers le Cloud Azure pour Circet
type: post
lang: fr
tags: [GitOps, finops, terraform, entreprise scale, azure]
image: /images/post/2024/11/circet-migration-cloud.png
summary: "Retour d'experience sur le déploiement d'entreprise scale landing zone"
---
## Introduction
Le projet Circet est une initiative ambitieuse visant à moderniser les mécanismes d'intégration de systèmes de l'entreprise en s'appuyant sur les principes d'entreprise service bus,
 notamment avec l'utilisation de Logic Apps. Ce projet comprend également la mise en ligne d'une API Métier de prises d'ordre de travail en mode low code et l'implémentation de processus RH.

## Objectif
Le projet vise à déployer une architecture de zone d'atterrissage à l'échelle de l'entreprise sur Azure, basée sur les recommandations et les meilleures pratiques de Microsoft.
Cette architecture modulaire est conçue pour évoluer avec les besoins de l'entreprise, qu'il s'agisse de la migration d'applications ou du déploiement de nouvelles applications sur Azure.

## Contexte
Circet est une entreprise spécialisée dans le déploiement et la maintenance d'infrastructures télécom mobiles et fixes pour des compagnies telles qu'Orange, Bouygues, SFR et Free.
Présente dans 13 pays, Circet investit également dans la transition énergétique. Le projet vise à améliorer l'expérience utilisateur, à augmenter le catalogue d'offres et à optimiser les processus de développement.
Circet a fait confiance à Orange Business pour réaliser les travaux. L'utilisation massive de l'automatisation via l'infrastructure as code a permis de construire une infrastructure cloud rapidement tout en respectant les best practices cloud.

## Architecture Globale
L'architecture de zone d'atterrissage de Circet repose sur le modèle Hub & Spoke, permettant une gestion efficace des ressources critiques sur Azure 
Cette architecture comprend plusieurs zones de conception, notamment la facturation Azure, la gestion des identités et des accès,
la continuité des activités et la reprise après sinistre, ainsi que la gestion des ressources de connectivité et de gestion.

## Zones de Conception
1. **Facturation Azure et Annuaire Active Directory Azure :** Gestion des abonnements et des contrats Azure.
2. **Ressources de Base :** Organisation des groupes de gestion et des abonnements, gouvernance de la sécurité et conformité.
3. **Gestion des Identités et des Accès :** Synchronisation des utilisateurs et des groupes entre Active Directory et Azure Active Directory.
4. **Zone de Connectivité :** Connexion entre l'infrastructure sur site et le cloud, utilisant des appliances Fortigate pour la sécurité et la gestion du trafic réseau.
5. **Zone de Gestion :** Surveillance et gestion des ressources Azure, centralisation des journaux et des événements, audit de l'infrastructure.
6. **Zone de Production API :** Déploiement d'applications basées sur des services PaaS et SaaS, intégration de l'application logicielle pour le traitement des commandes des clients de Bouygues Telecom.

## Conclusion
Le projet Circet représente une avancée significative dans l'amélioration de l'infrastructure IT de circet et la transition vers le cloud. En adoptant les meilleures pratiques de Microsoft et en utilisant une architecture modulaire,
Circet est bien positionnée pour répondre aux besoins croissants de ses clients et améliorer l'efficacité de ses processus de développement. Merci à eux d'avoir fait confiance à Orange Business pour cette belle réalisation.