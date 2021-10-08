# Cloud Adoption Plan

Now that the cloud adoption strategy and business outcomes have been identified, the next step is to plan for cloud adoption.

Microsoft’s [cloud adoption plan](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/plan/) provides broad, detailed guidance and approach in defining this plan, which will be used as a reference for Kyndryl’s cloud adoption plan.  

Kyndryl’s cloud adoption is customized to meet our organization and delivery model and hence may not be following all the steps listed in above cloud adoption plan, but will be in alignment with the CAF plan

Goal is to provide a broad guidance for solution architects to prepare their client towards cloud adoption and migration.

This plan phase can be broadly classified into following areas:   
1.	Organization Alignment.  
2.	Skills Readiness.  
3.	Digital Estate.  
4.	Build migration plan

<center><img style="height:80%;width:100%" src="../images/adoptionplan.png" /></center>


##	1.Organization Alignment
In order to realize the business outcomes identified during strategy phase, organization and the people should be aligned towards the business outcomes and the plan.   

True organizational alignment takes time and full organization alignment is not a required component of the cloud adoption plan. At the minimum the client should be guided towards having people accountable for cloud governance if they don’t have already.

Cloud Governance team provides the necessary checks and balances to ensure that cloud adoption doesn't expose the business to any new risks. When risks must be taken, this team ensures that proper processes and controls are implemented to mitigate or govern those risks

Cloud Adoption team which executes the technical tasks including discovery, design, migration etc. will be part of Kyndryl managed services team.

###	Long Term Organization Alignment
It will become important to establish long-term organizational alignment, especially as cloud adoption scales across the business and IT culture. Alignment is so important that an entire section has been dedicated to it in the [organize methodology](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/organize/) of the Cloud Adoption Framework. This is outside the scope of this document and given here as guidance.

###	1.1 Actions for Organization Alignment
1.	Validate with client if Cloud Governance team exists, and if not, guide the client in forming a cloud adoption team using [CAF organization alignment](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/plan/initial-org-alignment) as reference. 

2.	Validate with client if cloud governance team exists, and if not, guide the client in forming a cloud governance team

3.	It is important to emphasize that the both teams are knowledgeable with [Azure fundamentals, terminologies and portfolio](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/considerations/fundamental-concepts) including [5 R’s](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/digital-estate/5-rs-of-rationalization).

##	2.Skills Readiness
Roles, in the client organization, will change as client shift to cloud computing. For example, data center specialists might be replaced with cloud administrators or cloud architects. 

Even though Kyndryl, as the managed service provider, will be handling the technical tasks, there may still be employees in the client team who will be administrating and managing some aspects of the cloud, and hence these resources should be skilled up.

At the minimum the following actions should be performed, by the client, guided by the solution architect:   
1.	Start with review of Microsoft CAF’s [skill readiness plan](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/plan/adapt-roles-skills-processes).  
2.	Identify the responsibilities that is required for this transformation.  
3.	Identify skills required to support each responsibility.  
4.	Identify roles that will execute these skills, existing roles as well as new roles (for example: DevOps Architect).  
5.	Finally, guide the client team to acquire skills required for these specific roles thru’ [Microsoft Learn Catalog](https://docs.microsoft.com/en-us/learn/browse/).  

## 3.Digital Estate
Digital estate is the collection of IT assets that power business processes and supporting operations. 

**What to Measure**: Measurement of digital estate that the organization owns today is critical step required for planning and execution. What to measure in the digital estate depends on the type of business outcome identified as part of strategy: Migration, Innovation – Application & Data, and also Operational stability.  Review details for each of these explained in [CAF](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/digital-estate/#how-can-a-digital-estate-be-measured).  (**NOTE**: CAF calls this “how...”, though the contents talks about “what” should be the focus – VMs, Servers, etc)

**How to Measure**: Digital Estate Planning - There are various approaches on how to measure the digital estate:  
•	Top-Down workload driven approach.  
•	Bottoms-up asset-driven approach     
•	[Incremental approach](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/digital-estate/approach#incremental-approach) – **Recommended**   

**Inventory**: Following the incremental approach, the first step in planning the digital estate is gathering inventory.  
 Again, depending on the approach, inventory methods could vary from collecting inventory of servers, databases, networks etc, to identifying applications, APIs, data etc, as explained in CAF. There are various methods to perform inventory – Agent less, Agent based, Azure native tools like Azure Migrate and 3rd party discovery tools like Cloudscape

**Rationalize**: Once inventory is identified, the next step is evaluating the assets to determine the best approach to hosting them in the cloud. It is very difficult to rationalize at enterprise scale and hence recommended to align with the [incremental rationalization](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/digital-estate/rationalize#incremental-rationalization) approach prescribed in CAF.    

This incremental rationalization process starts by focusing on reduced or specific data points from the discovery, example focus on specific data centers, or regions to be closed, or critical application workloads that need to be scaled or a combination of all. It is possible to have more than one rationalization – example, re-host and retire for specific data centers, whereas use PaaS services for specific applications


###	3.1 Actions for Digital Estate Planning
1.	Ensure the client is aligned with the [incremental approach](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/digital-estate/approach#incremental-approach) for digital estate planning.  
2.	Gather Inventory through discovery. Kyndryl will follow **CMM (Cloud Migration & Modernization)** method for discovery & inventory which uses a combination of 3rd party tool, Cloudscape, home grown tool and Azure migrate for discovery and assessment. Refer to “[Cloud Migration & Modernization Capability Guide](https://w3.ibm.com/w3publisher/cloudmigrationfactory-offerings/capabilities-enablement/target-platforms-guides-handbooks)” for details.    
o	CMM performs the discovery and stores all the results in “Transition Manager” repository. This repository contains the following outputs:  
	Azure Discovery/Inventory output.   
	Azure Affinity/Dependency output.   
	Azure Application inventory.    

3.	Rationalize – identify, along with the client, specific workloads or data centers or application specific – and choose the best approach for migrating these to the cloud, as well as for retiring and releasing migrated assets.

##	4.Review Cloud Adoption Best Practices & Anti-Patterns
Microsoft’s CAF has provided few samples and [best practices](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/plan/contoso-migration-assessment) for cloud adoption. Review these best practices which will help in fine tuning the planning process.

It is also important to validate that the plan approach against [anti-patterns](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/antipatterns/plan-antipatterns), defined in CAF


##	5. Build the plan
Now that the inventory is available, and rationalized approach to cloud adoption has been identified, best practices and anti-patterns have been considered, next step is to start planning for the adoption.
 
1.	Define and prioritize workloads: Prioritize first 5-10 workloads to establish an initial adoption backlog. 
2.	Map assets to workloads: Identify which assets (proposed or existing) are required to support the prioritized workloads. 
3.	Review rationalization decisions: Review rationalization decisions to refine adoption path decisions: migrate or innovate. 
4.	Establish iterations and release plans: Iterations are the time blocks allocated to do work. Releases are the definition of the work to be done before triggering a change to production processes. 
5.	Estimate timelines: Establish rough timelines for release planning purposes, based on initial estimates

**REFERENCE**:  
a.	Additional planning details included in [CAF](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/plan/plan-intro#align-strategy-and-planning).    
b.	Planning Tool(s): Azure has a native tool, [Azure DevOps Services demo generator](https://docs.microsoft.com/en-us/azure/devops/demo-gen/), to assist with the planning which can be used for this purpose. Alternate planning tools can also be used by Kyndryl account team and the client. 

###5.1 Validate the plan – SMART Assessment
It is important to validate the plan and get an external view of how far the plan is actually ready for migration. Have all the elements on Azure ready to on-board/migrate the workload to Azure. 

Fortunately, Azure’s [SMART (Strategic Migration & Assessment Tool)](https://docs.microsoft.com/en-us/assessments/?mode=pre-assessment&id=Strategic-Migration-Assessment) provides an easy way to assess this readiness.

Perform the assessment and save/download the assessment results.

##	6. Update Strategy & Plan Template
All the above actions should be recorded for client validation as well as for auditing purposes.

Update [Kyndryl-Azure Strategy & Plan template](https://ibm.box.com/s/g82a595tk2k8oj73udsuv85w4nlsz00r) with the following, under “Cloud Adoption Plan” slide

1.	SMART Assessment Results Link: 
2.	Discovery/Inventory Links (from CMM):  
   a.Azure Discovery/Inventory output.  
   b.Azure Affinity/Dependency output.        
   	c.Azure Application inventory.       
   	d.Transition Manager Export. 
3.	Link to High level adoption plan

