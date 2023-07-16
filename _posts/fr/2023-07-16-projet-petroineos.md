---
layout: post
comments: true
title: Retour sur le projet Petroineos
type: post
lang: fr
tags: [GitOps, finops, terraform, entreprise scale, azure]
image: /images/post/2023/07/2023-07-16-retour-sur-le-projet-petroineos.png
summary: "Retour d'experience sur le déploiement d'entreprise scale landing zone"
---

# Le projet

Encore une histoire réussit avec Azure. Voilà maintenant un an et demi que j'interviens en tant que lead Developer et Architecte Azure dans ce beau projet de move to cloud. Petroineos est une entreprise basée à Martigues qui exploite une des plus grandes raffineries de France située à Lavera. Ce monde industriel est fortement challengé afin de se moderniser et d'apporter des innovations afin de garantir sur des sites Ceveso toujours plus de sécurité tout en contraignant l'inflation des coûts d'exploitation. L'enjeu de la valorisation des données et l'usage des nouvelles technologies de la Data et de l'IA imposent par ailleurs une montée en gamme des DSI de ce secteur industriel.

C'est avec ces nouveaux enjeux que la direction de Pétroineos s'est tournée vers Orange Business pour repenser la façon de produire son système d'information. Les attentes clés à l'origine du projet étaient:

- Assurer le déploiement sur Azure d'une application Web développée par un partenaire en python
- Mettre en ligne une plateforme Data et IoT afin de traiter et valoriser la donnée produite par les équipements de la raffinerie
- Déployer l'ensemble de ses nouveaux assets en respectant les best practices tout en embarquant la DSI vers ses nouvelles méthodes

J'ai donc proposé la mise en place au sein d'un tout nouveau tenant Azure d'une Entreprise Scale landing zone construite autour des zones plateformes suivantes:

1. d'une zone de connectivité
2. d'une zone de gestion d'identité
3. d'une zone de management
4. d'une zone devops

Complétés par les zones applicatives suivantes, comportant chacune une zone de production et de préproduction:

1. Une zone Application
2. Une zone Data

À toutes ces zones, nous avons ajouté une zone Sandbox afin que les équipes puissent disposer d'un espace d'apprentissage et de maquettage.


# Parlons architecture

Dans le cadre de ce projet, le groupe Petroineos faisait ses premiers pas dans le cloud, mais assez rapidement les ateliers Stratégie du processus [Cloud Adoption Framework](https://azure.microsoft.com/fr-fr/solutions/cloud-enablement/cloud-adoption-framework) ont montré qu'il fallait prévoir des zones d'extensions, car de futurs besoins allaient arriver.

C'est en ce sens que nous nous sommes appuyés sur le modèle de zone entreprise scale qui facilite l'extension de service applicatif tout en mettant en place des services communs mutualisés. L'architecture [Hub and Spoke](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?tabs=cli) mise en oeuvre avec un [Virtual Wan](https://learn.microsoft.com/en-us/azure/virtual-wan/virtual-wan-about) permet la mise en oeuvre mutualisée d'une connectivité On Prem tout en facilitant la mise en oeuvre des processus de sécurité (Azure Firewall, IDS, IPS, NGS, ...).

Si on parle applications, nous avons fait le choix de containeriser l'ensemble des applications afin de standardiser le processus de production logiciel tout en définissant une interface claire entre les développeurs et les OPS. Cette approche facilite la communication entre les deux équipes tout en proposant une première étape de modernisation vers K8s. Par ailleurs, containeriser c'est bénéficier d'un espace de modernisation fort sur l'architecture applicative, mais c'est une autre histoire.

Concernant le déploiement des zones applicatives, nous avons fait le choix des hoster dans un application plan. En effet au démarrage du projet, les équipes en chargent de l'exploitation étaient novices vis-à-vis de AKS et les containers apps n'étaient pas encore prêtent pour la production. Faire le choix de la maîtrise technique plutôt que la modernisation à toute force est souvent payant sur le long terme.

Vous trouverez ci-dessous une version très très high level de l'architecture, mais elle permet de comprendre l'idée générale de la Landing zone:

![hld](/blog/images/post/2023/07/2023-07-16-retour-sur-le-projet-petroineos-hld.png)

Afin d'adresser le processus finops nous avons mis en place une petite azure fonction qui alimente le datalake avec tous les coûts azure chaque jour. Simple et efficace le tableau de bord PowerBi évite le débordement des coûts en offrant au pilote de projet un retour rapide sur les dépenses engagées.

# Retour d'expérience

## S'appuyer sur un processus pour aller dans le cloud

Migrer dans le cloud n'est pas chose simple et souvent c'est difficile d'identifier par où commencer. En tant que consultant je conseille fortement de vous appuyer sur des processus
qui vous guide à chaque étape du projet. 

Le Cloud Adoption Framework et le Well Architected Framework proposé par Micrsocoft sont d'excellent point de départ. Je vous invite à ne pas négliger les étapes de stratégie et de planification. En effet, savoir où l'on veut aller, avec quelles équipes et quelles compétences au début du projet et souvent une garantie supplémentaire de succès.

## Pensez applications et usages avant technologie

Le cloud est un changement profond dans les organisations et pour garantir le bon déroulement des opérations il faut absolument prendre en considérations l'ensemble de la chaine de productions du SI en partant des applications pour aller vers l'infrastructure. Aussi impliquer les équipes de développement, comprendre leur processus de développement, leur méthode de versionning, leurs contraintes métiers est primordial pour proposer une architecture cloud efficiente.

## Inclure les équipes de tout horizon

S'assurer de la montée à bord de l'ensemble des équipes est essentiel. Vous devez inscrire dans le projet les développeurs et les opérations certes, mais n'oubliez pas les fonctionnels, les managers et bien évidemment la direction. Assurez-vous que les compétences pour gérer et exploiter le cloud sont présentes dans l'écosystème projet et le cas échéant mettez en place un plan de formation. Avec le projet cloud, l'entreprise Pétroineos a profondément changé pour se moderniser et se rendre plus agile. Les nouvelles pratiques vis-à-vis de la production du SI se déploient un peu plus chaque jour. C'est important de prendre en compte la courbe d'apprentissage de chacun.

## Automatiser, automatiser, et toujours automatiser

Aller dans le cloud, c'est avant tout mettre en place les pratiques d'automatisation. Sans ses processus vous perdez une large partie des bénéfices que vous apporte le cloud que ce soit dans la gestion finops de votre projet qu'en résilience et efficience des déploiements. Nous avons fait le choix d'utiliser terraform, et la méthode GitOps pour mettre en oeuvre le projet et quelques centaines de déploiements plus loin heureusement que nous n'avons jamais renoncé.

# Les résultats

Voici quelques chiffres:

- **2000+** c'est le nombre d'utilisateurs de nos différentes landing zones
- **300+** c'est le nombre de déploiements terraform
- **1500** c'est le nombre de ressources terraform déployées
- **100+** c'est le nombre de jours nécessaires pour construire la landing zone

Et plus que tous ces chiffres, voici le retour client qui a accepté de témoigner de la qualité du travail: [https://www.orange-business.com/fr/temoignage-client/petroineos-strategie-devops-dans-environnement-hybride](https://www.orange-business.com/fr/temoignage-client/petroineos-strategie-devops-dans-environnement-hybride).

