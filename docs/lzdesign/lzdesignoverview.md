# Landing Zone Design Overview


Purpose of this document is to guide Kyndryl solution architects to design Azure Landing Zone for Kyndryl managed accounts.

This document will provide guidance on critical aspects of Landing Zone design and standards that need to be followed.

Guidance in this document is based upon IBM Cloud Adoption Design (CAD) and Microsoft’s Cloud Adoption Framework (CAF).   The Cloud Adoption Framework is a full lifecycle framework, supporting customers throughout each phase of adoption. Guidance in this document focuses on the “Ready”, “Govern” and “Manage” phases.


<center><img style="height:100%;width:200%" src="../images/lzdesign-intro.png" /></center>
 
##	Landing Zone Design Scope
This **version** of the document will explain in detail the options and recommendations to design a landing zone design for a client, with focus on the below areas:						
 
a)	Landing zone design for hosting the platform services in support of the workloads and not the workloads themselves. This constitutes the Azure control and management plane for the landing zone.     
b)	General recommendations/best practices to be implemented. Specific details within each of the areas – for example: Policies, Role Definitions, Role Assignments, Scope, Monitoring Metrics are NOT included in this version. These will be added in future sprints.     
c)	Implementation (how to) details are NOT included in this version. They will be added in future sprints.     
d)	The goal of this document is to provide a template to capture all the design elements, which could then be used as the source for implementing Landing Zone through automation. This template will be provided in a future release.


##	Account Solution Document Template
Solution Architects should develop a solution doc for each account, which explains Landing Zone design, based on the guidelines and standards defined in this guide. This is an audit requirement as well as a compliance requirement that is part of Azure MSP certification

A [Solution Design template](https://kyndryl.box.com/s/haudsnaeb3deyn2cns75qetbwpbq3s3o) is provided which shall be used by the Solution Architects. 

