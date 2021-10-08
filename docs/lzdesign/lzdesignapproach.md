# Landing Zone Design Approach

An Azure landing zone results from a multi-subscription Azure environment that addresses scale, security governance, networking, and identity. Azure landing zones are pre-provisioned through code and enable application migration, modernization, and innovation at enterprise-scale in Azure. These zones consider all platform resources that are required to support the customer's application portfolio and do not differentiate between infrastructure as a service or platform as a service.

Azure CAF provides two approaches for implementing landing zones.

1.  **Start small and expand** -- The intent is to support low-risk workloads and with little or no focus on the Govern and Manage methodologies.

2.  **Enterprise-scale** -- The intent is to support mission-critical workloads and with comprehensive application of the Govern and Manage methodologies.

This document will focus on the enterprise-scale approach.

The rest of this section covers,

-   A component/services view of a Landing Zone, identifying all the parts that together compose a landing zone.

-   A brief description of the enterprise-scale architectural approach to the construction of landing zones.

-   Critical design areas to help translate requirements to Azure constructs and capabilities that in turn constitute a landing zone.

-   Enterprise-scale reference architectures for a landing zone based on three different network topologies.

-   Solution guidance in terms of the assets and steps involved to create a landing zone.

-   Design principles that guide the technical decisions across critical design areas.  

## Landing Zone -- Component/Services View

<center><img style="height:200;width:150" src="../images/designapproach-component-services-view.png" /></center>

The view in the above diagram shows multiple applications hosted in the landing zone using shared services for identity and access management, policies, network, management, and monitoring. Each application leveraged platform resources such as VMs, Storage, Databases and has application-specific identity and access management, policies, keys, monitoring and management agents.

## Enterprise-Scale Architecture

The enterprise-scale architecture provides prescriptive guidance coupled with best practices for the Azure control plane. It follows design principles across the critical design areas for an organization\'s Azure environment.

The enterprise-scale architecture is modular by design. It allows starting with a foundational landing zone control plane that support application portfolios, no matter whether the applications are being migrated or are newly developed and deployed to Azure. The architecture can scale alongside business requirements regardless of scale point.

## Critical Design Areas

The core of enterprise-scale architecture, as defined by CAF, contains a critical design path comprised of ***eight fundamental design areas*** with heavily interrelated and dependent design decisions.

Overview of the eight critical design areas are listed below:

| No.  | Design Areas                                                 | Objective                                                    |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | [Enterprise Enrolment](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/eaenrolment/) | For enterprise customers with an Azure commitment, proper tenant creation and enrollment is an important early step |
| 2    | [Identity & Access Management](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/iam/) | Identity and access management is a primary security boundary in the public cloud. It\'s the foundation for any secure and fully compliant architecture |
| 3    | [Network topology and connectivity](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/nwtopology/) | Networking and connectivity decisions are an equally important foundational aspect of any cloud architecture |
| 4    | [Resource organization](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/orgmgmt/) | As cloud adoption scales, considerations for subscription design and management group hierarchy have an impact on governance, operations management, and adoption patterns |
| 5    | [Governance disciplines](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/secgov/) | Automate auditing and enforcement of security governance and compliance policies |
| 6    | [Operations baseline](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/monitoringmgmt/) | For stable, ongoing operations in the cloud, an operations baseline is required to provide visibility, operations compliance, and protect and recover capabilities |
| 7    | [Business continuity and disaster recovery (BCDR)](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/bcdr/) | Resiliency is key for smooth functioning of applications. BCDR is an important component of resiliency. BCDR involves protection of data via backups and protection of applications from outages via disaster recovery |
| 8    | [Deployment options](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/platformauto/) | Align the best tools and templates to deploy your landing zones and supporting resources |


These fundamental design areas can be ***categorized as Foundational & Core elements*** from a solutioning perspective, as shown below.

<center><img style="height:300;width:200" src="../images/designapproach-foundational-core-elements.png" /></center>

This document provides **design guidance for each of the above eight critical design areas**, in separate sections. Each section provides guidance on design considerations, standard and optional recommendations to be followed.

## Reference Architectures

**Three reference architectures** are described below based on a particular network topology. Each extends the Component/Services view described previously into a deployment model showing the logical location of the various platform services.

### Enterprise-scale architecture -- Hub-spoke with Virtual Network Gateways for Small Enterprises

<center><img style="height:200;width:150" src="../images/designapproach-architecture-small-enterprises.png" /></center>


This reference architecture provides a design path and initial technical state for Small Enterprises to start with foundational landing zones that support their application portfolios. It is meant for organizations that do not have a large IT team and do not require fine grained administration delegation models. Hence, Management, Connectivity and Identity resources are consolidated in a single Platform Subscription. The connectivity resource group within the Platform subscription leverages the hub and spoke topology with Virtual Network gateways. In the event connectivity to customer premises is not needed, the connectivity resource group within the Platform subscription can be dispensed with. 

If the business requirements change over time, the architecture allows for creating additional subscriptions and placing them into the suitable management group and assigning Azure policies.

A detailed description of the various resources deployed in the above reference architecture can be obtained from [Deploy Enterprise-scale for small enterprises documentation](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/treyresearch/README.md).

### Enterprise-scale architecture -- Hub-spoke with Virtual Network Gateways for Medium and Large Enterprises

<center><img style="height:200;width:150%" src="../images/designapproach-architecture-hub-spoke.png"/></center>

This reference architecture is well suited for customers who want to start with Landing Zones for their net new deployment/development in Azure by implementing a network architecture based on the traditional hub and spoke network topology leveraging VPN/Express Route gateways. It is meant for organizations that have large, decentralised IT teams.

A detailed description of the various resources deployed in the above reference architecture can be obtained from [Deploy Enterprise-Scale with hub and spoke architecture documentation](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/adventureworks/README.md).

### Enterprise-scale architecture -- Hub-spoke with Virtual WAN for Medium and Large Enterprises

<center><img style="height:200;width:150" src="../images/designapproach-architecture-virtual-wan.png"/></center>

This reference architecture is also well suited for customers who want to start with Landing Zones for their net new deployment/development in Azure, where, in addition to the needs of large, decentralised teams, a global transit network is required, including hybrid connectivity to on-premises datacentres and branch offices via ExpressRoute and VPN.

A detailed description of the various resources deployed in the above reference architecture can be obtained from [Deploy Enterprise-Scale with Azure VWAN documentation](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/contoso/Readme.md).

### Other Architectures

Azure CAF provides a [set of implementation options](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/implementation-options) that can be leveraged for rapid creation of a landing zone. The options provided support combinations of the various options to add on functionality and services as needed. The options provided include the three reference architectures described above. A [User Guide](https://github.com/Azure/Enterprise-Scale/wiki/Deploying-Enterprise-Scale) exists for the implementation of the "Hub-spoke architecture with Virtual Network Gateways" architecture.

## Landing Zone Guidance Assets

The enterprise-scale approach to construct landing zones includes three sets of assets to support cloud solution teams:

-   Design guidelines derived from evaluation of requirements against critical design areas that drive the design of the enterprise-scale landing zone.

-   Reference architectures that demonstrate design areas and best practices.

-   Azure Resource Manager templates that enable rapid deployment of the reference architectures.(in a later release)

## Design Principles

The [principles](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/design-principles) in the table below serve as a compass for subsequent design decisions across critical technical domains.

-   **Subscription democratization** - Subscriptions should be used as a unit of management and scale.

-   **Policy-driven governance** -- Use Azure Policy to provide guardrails and ensure continued compliance of platform and applications.

-   **Single control and management plane** - unified and consistent control plane across all Azure resources and provisioning channels

-   **Application-centric and archetype-neutral** -- No differentiation between applications (old/new, IaaS/PaaS)

-   **Recommendations** -- Use preview services and service roadmaps to overcome technical blockers.
