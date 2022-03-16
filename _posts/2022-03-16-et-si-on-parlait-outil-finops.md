---
layout: post
comments: true
title: Et si on parlait outil finops
type: post
tags: [devops, cicd, finops, terraform, infracost]
image: /post/2022/03/2022-03-16-et-si-on-parlait-outil-finops.png
summary: "L'approche finops dans le cloud est indispensable pour être efficiente, mais elle doit être outillée. Infracost est là pour vous."
---

On parle souvent de mise en place d'une pratique finops lorsque l'on évoque des projets de migration dans le cloud qui ont réussi. En effet
rien de plus dramatique, mais hélas souvent trop fréquent, qu'une migration dans le cloud d'une infrastructure qui fait exploser les coûts de possession 
de cette dernière.

Si beaucoup d'équipes de développement et des DSI se disent finops, la pratique n'est hélas pas souvent largement répandue de façon exhaustive au sein des organisations,
car elle met en jeu un large périmètre d'activité. Je ne vais pas détaillé ici les différentes activités et bonnes pratiques du finops que vous pourrez consultez sur le site [finops.world/pratiques](https://finops.world/pratiques), mon intention est de mettre en lumières l'outil ***Infracost*** qui me semble une très bonne option quant on parle cloud, Automation, Terraform et azure.

## Le Finops "at early stage"

Ma conviction c'est que pour ne pas dépenser dans le cloud et maîtriser ses coûts d'infrastructure, il faut se donner très tôt, dans le cycle de déploiement, une vision des montants de consommation engagés. Avec l'automatisation, l'infrastructure as a code, les dérives sont rapides pour les raisons suivantes:

1. Les acteurs du développement d'infrascture sont souvent peu sensibilisés au coût de ce qu'ils déploient
2. Le processus de cotation des éléments déployés n'est pas inclus dans le cycle de vie du projet
3. Le mode de facturation des éléments d'infrastructure est souvent complexe 

Sans connaissance des coûts, sans actualisation fréquente, les écarts entre le budget estimé et la facture réelle peuvent être importants.

Afin d'éviter ces écueils, je me suis rendu compte qu'il était primordial de s'outiller afin:

1. D'effectuer des cotations d'infrastructure à haute vélocité
2. De doter les équipes d'infrastructure de métriques financière le permettant très tôt de constater des dérives de coûts
3. De mettre l'ensemble de l'outillage dans l'écosystème du développeur d'infrastructure

En effet rien de mieux que de mettre dans les mains de celui qui génère les coûts (le codeur terraform) les outils qui l'alertent sur la facturation qu'il va générer.

Aussi, je voulais vous mettre en lumière l'outil [infracost](https://infracost.io) qui peut être une réponse à vos attentes. À défaut de répondre à toutes vos attentes en termes de finops, il vous permettra d'éviter les hémorragies de $$.

## Qu'y a-t-il dans la boite Infracost

Le projet [infracost](https://github.com/infracost/infracost) est un projet open source qui couvrent les trois grands hyperscaler que sont [Azure](https://azure.microsoft.com/en-us/), [AWS](https://aws.amazon.com/fr/) et [GCP](https://cloud.google.com/?hl=fr).

Infracost permet:

1. D'évaluer sur le poste du développeur terraform le coût de l'infrastructure dès la phase de plan (autrement dit avant le déploiement)
2. D'effectuer une analyse en delta des coûts entre deux déploiements
3. D'effectuer une analyse au sein de votre chaine Dev/Ops
4. De publier des rapports de coûts au format HTML

Le tout en mode SaaS (utilisation des apis et de la base de prix fournis par Infracost) ou en mode OnPrem (moyennant le déploiement d'un serveur Node, et d'une base de données Postgres)

## Liens utiles

Quelques liens pour aller plus loin:

- [Web Site Infracost](https://www.infracost.io/)
- [Repository GitHub Infracost](https://github.com/infracost/infracost)
- [Lien vers la documentation du produit](https://infracost.io/docs/)
- [Ressources supportées sur Azure](https://www.infracost.io/docs/supported_resources/azure/)
- [Ressources supportées sur AWS](https://www.infracost.io/docs/supported_resources/aws/)
- [Ressources supportées sur GCP](https://www.infracost.io/docs/supported_resources/gcp/)
