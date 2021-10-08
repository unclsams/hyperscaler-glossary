# Overview
This section walks through the design of a Landing Zone using a sample workload scenario.

Design [approach](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/lzdesignapproach/) and recommendations described in the ["landing zone critical design areas"](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/lzdesignapproach/#critical-design-areas) from previous section will be applied against this workload to define the landing zone.

It is to be noted that the intent is to emphasize the design approach and not to focus on the correctness of the solution that is arrived at. The plan is to validate the proposed solution as and when sufficient resources are available.

## Design Approach

The approach consists of addressing the critical design areas outlined in the [Design document](https://pages.github.kyndryl.net/OCTO/azure/lzdesign/lzdesignapproach/), one after the other. Design considerations and recommendations are considered against the requirements that are either stated in the [Workload description](./wldescription.md) or assumed where needed. The outcome are design decisions that then drive the solution design.   

Practically, the approach may be more iterative to address the possible inter-dependencies between these areas.

## Workload Description

Two incarnations of a three tier Web application, consisting of Web, Application and Database tiers, are considered as workload for the purpose creating a landing zone design. One incarnation is that of a J2EE application that leverages a load balancer only for the Web tier. The other is a .NET application that uses load balancers for both the Web and Application tiers.  

Two environments are considered – one for Dev/Test and the other for Production. Both environments need to be isolated from each other. 
Connectivity to the cloud environments from customer premises is needed for configuration of the cloud resources and management of the applications.   

The concurrent users and aggregate transaction rate in terms of the workload scalability and performance are not considered in this exercise. So, sizing of the solution components in the proposed design is not discussed.



