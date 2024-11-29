---
layout: post
comments: true
title: Success of the Migration to Azure Cloud for Circet
type: post
lang: fr
tags: [GitOps, finops, terraform, entreprise scale, azure]
image: /images/post/2024/11/circet-migration-cloud.png
summary: "Retour d'experience sur le déploiement d'entreprise scale landing zone"
---
## Introduction
The Circet project is an ambitious initiative aimed at modernizing the company's system integration mechanisms by leveraging enterprise service bus principles,
 particularly with the use of Logic Apps. This project also includes the deployment of a low-code business API for work order management and the implementation of HR processes.

## Objective
The project aims to deploy an enterprise-scale landing zone architecture on Azure, based on Microsoft's recommendations and best practices.
 This modular architecture is designed to evolve with the company's needs, whether it involves migrating applications or deploying new applications on Azure.

## Context
Circet is a company specializing in the deployment and maintenance of mobile and fixed telecom infrastructures for companies such as Orange, Bouygues, SFR, and Free.
 Present in 13 countries, Circet also invests in energy transition. The project aims to improve user experience, increase the catalog of offers, and optimize development processes.
  Circet entrusted Orange Business to carry out the work. The extensive use of automation via infrastructure as code allowed for the rapid construction of a cloud infrastructure while adhering to cloud best practices.

## Overall Architecture
Circet's landing zone architecture is based on the Hub & Spoke model, allowing for efficient management of critical resources on Azure.
 This architecture includes several design areas, including Azure billing, identity and access management, business continuity and disaster recovery, as well as connectivity and management resources.

## Design Areas
1. **Azure Billing and Azure Active Directory:** Management of Azure subscriptions and contracts.
2. **Core Resources:** Organization of management groups and subscriptions, security governance, and compliance.
3. **Identity and Access Management:** Synchronization of users and groups between Active Directory and Azure Active Directory.
4. **Connectivity Zone:** Connection between on-premises infrastructure and the cloud, using Fortigate appliances for security and network traffic management.
5. **Management Zone:** Monitoring and management of Azure resources, centralization of logs and events, infrastructure auditing.
6. **API Production Zone:** Deployment of applications based on PaaS and SaaS services, integration of the software application for processing Bouygues Telecom customer orders.

## Conclusion
The Circet project represents a significant advancement in improving Circet's IT infrastructure and transitioning to the cloud.
 By adopting Microsoft's best practices and using a modular architecture, Circet is well-positioned to meet the growing needs of its clients and improve the efficiency of its development processes.
  Thanks to them for trusting Orange Business for this great achievement.