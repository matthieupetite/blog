---
title: Cloud Engineer
tags: [iptv, reseau, déploiement, numericable, adsl, supervision]
image: /images/experiences/swatchgroup-1.png
compagny: Orange Application For Buiness - Swatchgroup
summary: New experience as Cloud Engineer
type: experience
start: Decembre 2007
end: Juin 2007
order: 8
logo: swatch.png
lang: en
---

As part of a product development for the world of watchmaking, I am in charge of building the lifecycle management application solution which covers on the one hand the application deployment (Microservice-based solution), the deployment of the Kubernetes infrastructure (~30 clusters worldwide) and finally the functional (Cluster ELK) and technical (Prométhéus / Elk / Grafana / Thanos deployment) supervision infrastructure.

The delivered forge therefore contains all the application builds and builds linked to the IaC, as well as the release pipeline to deploy, worldwide, the K8s clusters behind an Azure Traffic Manager.

The forge is built in such a way that all the platforms (dev, integration, UAT, etc.) are built automatically, by injecting by configuration, the different needs in terms of topology.

The service covered upstream the definition of the Azure infrastructure, but also the selection of the mode of management of the K8s clusters within the forge. So I opted for the use of Chart Helm to facilitate deployments.

Finally, I implemented the load testing solution in order to validate the horizontal auto scaling processes defined within the Helm charts.

From an operational point of view, I lead a team of 4 people in charge of the construction of infrastructure and supervision elements

Technical and methodological environments:

1. Docker, Helm, Azure ACR,
2. Azure Dev/Ops, Azure (IaaS), Azure (PaaS), ARM, Az Cli, Powershell
3. Java,
4. Linux,
5. Azure VPN, Application Gateway, Traffic Manager,
6. Microsoft Load Testing,
7. Grafana, Prometheus, Thanos, CouchDB, …