---
layout: post
comments: true
title: System modernization at Thiriet, a case study
type: post
lang: en
tags: [GitOps, finops, terraform, entreprise scale, azure]
image: /images/post/2024/01/thiriet-migration-cloud.png
summary: "Review of the thiriet projet."
---

# Digital Transformation at Thiriet: A Case Study by a Cloud Architect from Orange Business Services

## Introduction

In the ever-evolving world of cloud technology, modernizing information systems is a crucial challenge for companies wishing to remain competitive. My recent experience as a cloud computing architect at Orange Business Services, specializing in Microsoft Azure technologies, offers a fascinating insight into this transformation. This blog post focuses on the project of migrating and modernizing the telephone order-taking system for Thiriet, a complex and enriching challenge as it involves modernizing one, if not the main, business application of the group. This application generates several hundred million euros in turnover, and the number of users is very significant.

## Project Context

Faced with the need to modernize its business applications and improve the efficiency of its operations, Thiriet turned to Orange Business Services for a cloud solution. The project included migrating six business applications, written in various languages like Java, NodeJs, .Net, and Crystal Report, to an Azure environment. To be sustainable, the project had to rely on a gradual migration of this set of applications, ensuring a gradual onboarding of the group's application development and sysops teams. Financial and regulatory aspects were, of course, very important, especially in the retail world where pressure on margins is significant and the context of rising energy costs represented a major challenge for this specialist in the cold chain.

## Challenges and Solutions

From our first discussions, the client understood that the main lever for long-term cost reduction required a coordinated approach to migration towards PAAS hosting scenarios. Azure offers a wide range of services that could be orchestrated to provide quality, high-performance, and secure service. The main challenges were:

1. **Application Modernization**: The first challenge was the containerization of applications for execution within App Service, which offers a first approach to container management without the difficulty of managing Kubernetes, out of reach for Thiriet's IT department. This step required a revision of key functionalities of some applications such as session management and dependency injection mechanism.

2. **Print Management**: A particular challenge was maintaining the ability to handle 200,000 paper prints per day while migrating to the cloud. Constructing a print management API, accessible through network hybridization, was the solution.

3. **Adoption of Microsoft's CAF Process**: The use of the Cloud Adoption Framework (CAF) was essential to ensure a successful transition, both technically and financially. Without this approach, project framing would have been difficult with dispersion of energy both in the design and deployment phases.

4. **Multi-Tenancy Strategy**: We reviewed the multi-tenancy strategy to allow the sharing of application instances across 82 call centers.

5. **Data Synchronization**: Real-time synchronization of business data from the Azure cloud to internal systems was another crucial aspect of the project. Real-time SQL data synchronization and management of order files (> 10,000/day) represented the main challenge.

## Results and Benefits

The migration to Azure brought several significant benefits:

1. Productivity Gain: thanks to the automation of deployments and administrative tasks.

2. Financial Efficiency: a notable reduction in the total cost of ownership.

3. Improved Availability: a drastic reduction in incidents and a shift in support requests from corrections to evolutions.

4. Methodological Transformation: adoption of DevSecOps and agility practices by development and infrastructure teams, supported by the implementation of the Azure DevOps toolchain.

## Conclusion

The modernization project at Thiriet has improved the technical infrastructure, but has also led to a significant cultural change. This experience underscores the importance of strategic planning and effective management to succeed in cloud migration. For companies considering a similar transformation, it is crucial to rely on a competent partner, favor a modernization approach, and fully commit to the process both technically and managerially.

Thanks to the Thiriet teams for trusting me with this achievement, which has been one of my best realizations in 2023.
