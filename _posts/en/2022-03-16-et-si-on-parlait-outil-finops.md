---
layout: post
comments: true
title: Et si on parlait outil finops
type: post
tags: [devops, cicd, finops, terraform, infracost]
image: /images/post/2022/03/2022-03-16-et-si-on-parlait-outil-finops.png
lang: en
summary: "L'approche finops dans le cloud est indispensable pour être efficiente, mais elle doit être outillée. Infracost est là pour vous."
---

We often talk about setting up a finops practice when we talk about successful cloud migration projects. In effect
nothing more dramatic, but unfortunately often all too frequent, than a migration to the cloud of an infrastructure that causes the cost of ownership to skyrocket
of the latter.

Although many development teams and CIOs call themselves finops, the practice is unfortunately not often widespread in an exhaustive way within organizations,
because it involves a wide scope of activity. I will not detail here the different finops activities and good practices that you can consult on the site [finops.world/pratiques](https://finops.world/pratiques), my intention is to highlight the tool ***Infracost*** which seems to me a very good option when it comes to cloud, Automation, Terraform and azure.

## Finops "at early stage"

My conviction is that in order not to spend in the cloud and to control your infrastructure costs, you have to give yourself a vision very early in the deployment cycle of the amounts of consumption involved. With automation, infrastructure as a code, drifts are rapid for the following reasons:

1. Infrastructure development actors are often unaware of the cost of what they deploy
2. The process of rating deployed items is not included in the project life cycle
3. The billing method for infrastructure elements is often complex

Without knowledge of the costs, without frequent updating, the differences between the estimated budget and the actual invoice can be significant.

In order to avoid these pitfalls, I realized that it was essential to equip myself in order to:

1. Perform high-velocity infrastructure quotes
2. Provide infrastructure teams with financial metrics allowing them to see cost drifts very early on
3. To put all the tools in the ecosystem of the infrastructure developer

Indeed, nothing better than putting in the hands of the person who generates the costs (the terraform coder) the tools that alert him to the invoicing he will generate.

Also, I wanted to highlight the [infracost](https://infracost.io) tool which may be an answer to your expectations. Failing to meet all of your finops expectations, it will save you from loosing $$.

## What's in the Infracost box

The [infracost](https://github.com/infracost/infracost) project is an open source project that covers the big three hyperscalers that are [Azure](https://azure.microsoft.com/en-us/) , [AWS](https://aws.amazon.com/fr/) and [GCP](https://cloud.google.com/?hl=fr).

Infracost allows:

1. Assess the cost of the infrastructure on the terraform developer's position from the planning phase (in other words before deployment)
2. Perform a cost delta analysis between two deployments
3. Perform an analysis within your Dev/Ops chain
4. Publish cost reports in HTML format

All in SaaS mode (use of APIs and the price base provided by Infracost) or in OnPrem mode (by deploying a Node server and a Postgres database)

# Useful links

Some links to go further:

- [Web Site Infracost](https://www.infracost.io/)
- [Repository GitHub Infracost](https://github.com/infracost/infracost)
- [Lien vers la documentation du produit](https://infracost.io/docs/)
- [Supported resources Azure](https://www.infracost.io/docs/supported_resources/azure/)
- [Supported resources AWS](https://www.infracost.io/docs/supported_resources/aws/)
- [Supported resources GCP](https://www.infracost.io/docs/supported_resources/gcp/)

