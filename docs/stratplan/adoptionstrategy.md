# Cloud Adoption Strategy

There are **four steps** to be followed to understand client needs and define the cloud adoption strategy. Cloud adoption strategy can then be mapped to specific cloud capabilities and business strategies to meet the desired state.

<center><img style="height:100%;width:200%" src="../images/cloudadoptionstrat.png" /></center>

This section provides brief on each of these **four steps** and the **actions** need to be taken to define the strategy. For reference, details on the four areas are available in [CAF](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/strategy/).

It is important this effort should be performed **along with client stakeholders**, and the stakeholder names should be documented.


##	1.Define Motivation 

Business transformations that are supported by cloud adoption can be driven by various motivations.  It is possible and likely that several motivations apply at the same time. Microsoft CAF provides a list of relevant motivations that should be considered, at the minimum.  


CAF broadly classifies the motivations into three areas:  
•	Critical Business Event  
•	Migration
•	Innovation 

There may be additional motivations for a specific account(s), in which case, map those motivation to one of the three classifications.

###1.1 ACTION – Document Motivations & Classifications
It is important for the solution architect to document the motivations and the priority along with client stakeholders. 

Update the [Kyndryl-Azure Strategy & Plan template](https://ibm.box.com/s/g82a595tk2k8oj73udsuv85w4nlsz00r) to document the [motivations](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/strategy/motivations#motivations):  
1)	Identify the list of motivations, using CAF motivations as the starting point.   
2)	Prioritize each motivation into High, Medium & Low, based on business needs.   
3)	Identify which classification has the highest priority for the business. In the example below Data Center exit has got the high priority and hence critical business event classification shall be given priority, over others.   


| Classification| Motivations with priority |
|-----------------|----------------|
| Critical Business Event| 1.	Data center exit (H), 2.	Regulatory Compliance (M) |
| Migration| 1.	Cost Savings (M), 2.	Capex Reduction (M), 3.	Reduce Carbon Footprint(L) |
| Innovation| 1.	Scaling to meet geographic demands (M), 2.	Other justifications (L) |


The identified priority from above helps in driving the conversation towards the next set of actions, including choosing the first adoption project, which is explained later in this chapter.


##2.Define Business Outcomes
The next step is to help customers identify **desired** business outcomes that are concise, defined, and drive observable results or change in business performance, supported by a specific measure. Review [business outcomes](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/strategy/business-outcomes/) in Microsoft CAF, to learn and understand this topic.

After gaining an understanding of business outcomes, review [how to document business outcomes](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/strategy/business-outcomes/business-outcome-template).  

###2.1 ACTION - Document Business Outcomes
Document the business outcomes in [Kyndryl-Azure Strategy & Plan template](https://ibm.box.com/s/g82a595tk2k8oj73udsuv85w4nlsz00r)

At the minimum document the following in the template, sample included in above template:    
o	Key business outcomes 
o	Key capabilities required  
o	KPIs (if data available)

<center><img style="height:60%;width:80%" src="../images/businessoutcomesample.png" /></center>


##	3. Define Business Case/Justification
A business case provides a view of the technical and financial timeline of your environment and can represent the opportunities for reinvestment into further modernization. Developing a business case includes building a financial plan that takes technical considerations into account and aligns with business outcomes

Business case should consider [financial](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/strategy/achieve-more) aspects, environment scope, projected savings and many more. Use the [business case](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/strategy/cloud-migration-business-case) guidance provided in CAF, as the starting point.

In addition, there are native tools in Azure that could be used for estimating costs - [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator) and [Total Cost of Ownership (TCO) & ROI Calculator](https://azure.microsoft.com/pricing/tco/calculator/)

Technical Considerations: When making the shift to the cloud, there are technical considerations around how it will help improve manage and maintain cloud and workloads. This guidance will help discover the technical flexibility, efficiencies, capabilities and various options for cost optimization which aren’t possible with on-premises IT infrastructure. Review the [technical considerations](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/strategy/financial-considerations/) which will help the business case to migrate to the cloud. 

###3.1 ACTION - Document Business Justification – Technical & Financial

Document business justification in the in the [Kyndryl-Azure Strategy & Plan template](https://ibm.box.com/s/g82a595tk2k8oj73udsuv85w4nlsz00r)

1)	This section in the above template should list tangible and intangible benefits to the client by adopting the cloud strategy and the capabilities from Azure.  

Some examples are listed below, and more can be found in CAF links above:

* Estimated Cost Savings
* Reduced Data Center Footprint 
* Increased Productivity & Service Delivery

2)	Use TCO calculator to estimate the TCO and ROI and add these links in the Kyndryl-Azure Strategy & Plan template


## 4.Choose First Adoption Project
The first adoption project should align with the motivations behind cloud adoption. Whenever possible, the first project should also demonstrate progress towards a defined business outcome.  
Refer to the priority that was derived in the motivations **ADD LINK** section. Based on the priorities assigned to each of the motivations, one of the three classifications would result in higher priority. Below table provides guidance on the action to be taken based on the classification.

| Motivation Classifications & Priority| Action |
|-----------------|----------------|
| Critical Business Event| Accelerated migration or adoption at the earliest possible timeframe, in parallel with strategy & planning|
| Migration| Strategy & Planning |
| Others (Optional)  - if there are other business motivations not fitting into the above classification, define a new one. Example is “Operational Stability”|  |

Below are some sample actions for guidance:  

1)	If the priority is to respond to a critical business event, consider implementing Azure Site Recovery and set it up as a disaster recovery tool for the first project to quickly reduce current dependencies within the data center.   
2)	If a server migration has priority, consider starting with the migration of a noncritical workload and leveraging the Azure Readiness Guide and the Azure Migration Guide  as guidance for the migration of that first workload.  
3)	If the priority is to innovate or modernize current application, consider creating a targeted dev/test environment for your application.


###4.1	ACTION – Identify & Document First adoption Project
Based on above guidelines, identify an application or set of workloads for initial adoption, which could be considered a Proof-of-Concept (PoC).
For example, if Critical Business Event classification had high priority, then identify an application for which Azure Site Recovery can be implemented.

Document the first adoption project in [Kyndryl-Azure Strategy & Plan template](https://ibm.box.com/s/g82a595tk2k8oj73udsuv85w4nlsz00r).
