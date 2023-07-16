---
layout: post
comments: true
title: Retour sur le projet Petroineos
type: post
lang: en
tags: [GitOps, finops, terraform, entreprise scale, azure]
image: /images/post/2023/07/2023-07-16-retour-sur-le-projet-petroineos.png
summary: "Retour d'experience sur le déploiement d'entreprise scale landing zone"
---

# The project

Another success story with Azure. I've been working as Lead Developer and Azure Architect in this beautiful move to cloud project for a year and a half now. Petroineos is a company based in Martigues which operates one of the largest refineries in France located in Lavera. This industrial world is strongly challenged in order to modernize and bring innovations in order to guarantee ever more safety on Ceveso sites while constraining the inflation of operating costs. The issue of data enhancement and the use of new Data and AI technologies also require CIOs in this industrial sector to move upmarket.

It is with these new challenges that the management of Pétroineos turned to Orange Business to rethink the way of producing its information system. The key expectations driving the project were:

- Ensure the deployment on Azure of a web application developed by a partner in python
- Put online a Data and IoT platform in order to process and enhance the data produced by the equipment of the refinery
- Deploy all of its new assets while respecting best practices while onboarding the DSI towards its new methods

I therefore proposed the implementation within a brand new Azure tenant of an Enterprise Scale landing zone built around the following platform zones:

1. of a connectivity zone
2. an identity management zone
3. a management area
4. a devops area

Completed by the following application areas, each with a production and pre-production area:

1. An Application area
2. A Data area

To all of these areas, we've added a Sandbox area so teams can have a space to learn and model.


# Let's talk about architecture

As part of this project, the Petroineos group was taking its first steps in the cloud, but fairly quickly the Process Strategy workshops [Cloud Adoption Framework](https://azure.microsoft.com/fr-fr/solutions/cloud-enablement/cloud-adoption-framework) showed that it was necessary to plan areas of extensions, because future needs were going to arise.

It is in this sense that we relied on the enterprise scale zone model which facilitates the extension of application services while setting up shared common services. The [Hub and Spoke](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?tabs=cli) architecture implemented with a [Virtual Wan](https://learn.microsoft.com/en-us/azure/virtual-wan/virtual-wan-about) allows the shared implementation of On Prem connectivity while facilitating the implementation of processes security (Azure Firewall, IDS, IPS, NGS, ...).

If we talk about applications, we have chosen to containerize all the applications in order to standardize the software production process while defining a clear interface between the developers and the OPS. This approach facilitates communication between the two teams while offering a first step in modernizing towards K8s. Moreover, containerizing means benefiting from a strong modernization space on the application architecture, but that's another story.

Regarding the deployment of application areas, we have chosen to host in an application plan. Indeed, at the start of the project, the teams in charge of operation were new to AKS and the container apps were not yet ready for production. Choosing technical mastery rather than full-force modernization often pays off in the long run.

Below is a very very high level version of the architecture, but it helps to understand the general idea of ​​the Landing zone:

![hld](/images/post/2023/07/2023-07-16-back-to-the-petroineos-project.png)

In order to address the finops process we implemented a small azure function which feeds the datalake with all the azure costs every day. Simple and effective, the PowerBi dashboard avoids cost overflow by offering the project manager a quick return on the expenses incurred.

# Feedbacks

## Rely on a process to move to the cloud

Migrating to the cloud is not easy and often it is difficult to identify where to start. As a consultant, I strongly advise you to rely on processes
that guides you through every step of the project.

The Cloud Adoption Framework and the Well Architected Framework offered by Micrsocoft are excellent starting points. I urge you not to neglect the strategy and planning stages. Indeed, knowing where you want to go, with which teams and which skills at the start of the project is often an additional guarantee of success.

## Think applications and uses before technology

The cloud is a profound change in organizations and to guarantee the smooth running of operations it is absolutely necessary to take into consideration the entire IS production chain, starting from applications and moving towards the infrastructure. Also involving the development teams, understanding their development process, their versioning method, their business constraints is essential to offer an efficient cloud architecture.

## Include teams from all walks of life

Ensuring that all teams board is essential. You must include the developers and the operations in the project, of course, but don't forget the functional staff, the managers and of course the management. Make sure that the skills to manage and operate the cloud are present in the project ecosystem and, if necessary, set up a training plan. With the cloud project, the Pétroineos company has profoundly changed to modernize and become more agile. New practices with regard to the production of the IS are being deployed a little more every day. It's important to take everyone's learning curve into account.

## Automate, automate, and always automate

Going into the cloud is above all about setting up automation practices. Without its processes, you lose a large part of the benefits that the cloud brings you, whether in the finops management of your project or in the resilience and efficiency of deployments. We made the choice to use terraform, and the GitOps method to implement the project and a few hundred deployments later, fortunately we never gave up.

# The results

Here are some figures about the project:

- **2000+** is the number of users of our different landing zones
- **300+** is the number of terraform deployments
- **1500** is the number of terraform resources deployed
- **100++** is the number of days needed to build the landing zone

And more than all these figures, here is the customer feedback who agreed to testify to the quality of the work: [https://www.orange-business.com/fr/temoignage-client/petroineos-strategie-devops-dans-environnement -hybrid](https://www.orange-business.com/fr/testimonial-client/petroineos-strategie-devops-dans-environnement-hybride).
