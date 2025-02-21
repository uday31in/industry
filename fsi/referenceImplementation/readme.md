# Financial Services Industry (FSI) Landing Zones on Microsoft Azure - User Guide

![fsilzoverview](../docs/fsilandingpage.png)

This user guide explains the FSI Landing Zones on Microsoft Azure reference implementation, what it is, what it does, and how FSI organizations can use this as a starting point in their tenant - both for greenfield and brownfield deployments, to achieve a secure-by-default, and scalable Azure platform and landing zones regardless of their scale-point.

> Note: This reference implementation has been developed, validated, and proven with several of Microsoft's largest FSI customers, and represent the best practices for the FSI industry to accelerate a safe and secure by-default digital transformation for the organization as a whole on Microsoft Azure. We will continue to enhance and develop the reference implementation alongside with the overall Azure roadmap, based on proven and validated design patterns with FSI customers at scale.

## Table of Contents

- [What is FSI Landing Zones on Microsoft Azure reference implementation?](#what-is-fsi-landing-zones-on-microsoft-azure-reference-implementation)
- [Pricing](#pricing)
- [What if I already have an existing Azure footprint?](#what-if-i-already-have-an-existing-azure-footprint)
- [How FSI Landing Zones reference implementation works](#how-fsi-landing-zones-reference-implementation-works)
- [Deployment instructions](#deployment-instructions)
  - [Pre-requisites](#pre-requisites)
  - [Step-by-step deployment guidance](#step-by-step-deployment-guidance)

---

FSI Landing Zones on Microsoft Azure reference implementation provides prescriptive guidance coupled with Azure best practices for the financial services industry, and provides the optimial starting point with regards to security, governance, and compliance with the secure by-default principle, across many Azure services. This effort has been made to accelerate the FSI's journey to cloud, by adhering to Azure and security best practices for Azure services.

>Note: To read and learn about the architecture and design, please visit this [article](../docs/architectureAndDesign.md).

The reference architecture is modular by design and allows organizations of any size in the financial services industry to start with the optimized landing zones that support their banking workloads, and application portfolios.

In particular, it enables the organizations to start as small as needed and scale alongside their business requirements regardless of scale point.

## What is FSI Landing Zones on Microsoft Azure reference implementation?

The FSI Landing Zones reference implementation is an optimized, proven, authoritative, and roadmap aligned architecture that enables the financial services industry to deploy mission-critical, secure-by-default workloads in Azure at scale.

The reference implementation ties together all the Azure platform primitives and creates a proven, well-defined Azure architecture based on a multi-subscription design, leveraging native platform capabilities to simplify the operations of the platform as well as the landing zones containing the workloads.

## Pricing

There’s no cost associated with the reference implementation itself, as it it provides the architecture and controls that are constructed using existing Azure products and services. Therefore you only pay for the Azure products and services that you choose to enable, and also the products and services your organization will deploy into the landing zones for your workloads.

For example, you don’t pay for the Management Groups or the Azure policies that are being assigned, but assigning a policy to enable Microsoft Defender for Cloud (previously known as Azure Security Center Standard) on all landing zone subscriptions will generate cost on those subscriptions for the Microsoft Defender for Cloud service when the actual service is being created as detailed [here](https://azure.microsoft.com/pricing/details/azure-defender/).

## What if I already have an existing Azure footprint?

FSI Landing Zones reference implementation will meet the organizations in the financial services industry where they are, and the design has catered for existing subscriptions and workloads in Azure.

See the following [generic guidance](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/transition) to learn more how you can transition into an Azure architecture based on a multi-subscription design with clear separation of platform and landing zones, such as FSI Landing Zones on Microsoft Azure.

## How FSI Landing Zones reference implementation works

This section describes at a high level how FSI Landing Zones reference implementation works. Your landing zones are the output of a multi-subscription environment for all your Azure services, where compliance, guardrails, security, networking, and identity is provided at scale by the platform.

### FSI Landing Zones architecture

The Management Group structure implemented with FSI Landing Zones on Microsoft Azure is with clear intention to streamline and optimize operational excellence with security and governance being front and center. It caters for the entire lifecycle of a subscription, provides clear separation of concerns with regards to platform resources, and workload resources inside the landing zones.

The following structure is being created when deploying FSI Landing Zones on Microsoft Azure, and enables organizations to transition existing subscription into the target architecture, as well as being the ideal starting point for FSI organizations who are new to Azure.

![FSI Landing Zones management group structure](../docs/fsimgmt.png)

- **Intermediate Management Group** (directly under the tenant root group) is created with a prefix provided by the organization, which purposely will avoid the usage of the root group to allow organizations to move existing Azure subscriptions into the hierarchy, and also enables future scenarios. This Management Group is parent to all the other Management Groups created by FSI Landing Zones

- **Platform:** This Management Group contains all the *platform* child Management Groups, such as Management, Connectivity, and Identity. Common Azure Policies for the entire platform is assigned at this level

- **Management:** This Management Group contains the dedicated subscription for management, monitoring, and security, which will host Azure Log Analytics, Azure Automation, Azure Storage Account for NSG Flow Logs, and Microsoft Sentinel. Specific Azure policies are assigned to harden and manage the resources in the management subscription.

- **Connectivity:** This Management Group contains the dedicated subscription for connectivity for the Azure platform, which will host the Azure networking resources required for the platform, such as Azure Virtual WAN/Virtual Network for the hub, Azure Firewall, Azure Private DNS Zones, DDoS Protection plan, ExpressRoute/VPN Gateways among others. Specific Azure policies are assigned to harden and manage the resources in the connectivity subscription. For typical scale-out scenarios for the FSI industry, additional connectivity subscriptions can be added and brought to compliant state in an autonomous fashion due to the policy driven guardrails design principle.

- **Identity:** This Management Group contains the dedicated subscription for identity, which is a placeholder for Windows Server Active Directory Domain Services (AD DS) VMs, or Azure Active Directory Domain Services to enable AuthN/AuthZ for workloads within the landing zones. Specific Azure policies are assigned to harden and manage the resources in the identity subscription.

- **Landing Zones:** This is the parent Management Group for all the landing zone subscriptions and will have workload agnostic Azure Policies assigned to ensure workloads are secure and compliant.

- **** This is the dedicated Management Group for landing zones, which are optimized and governed aligned with migration scenarios, net-new development using PaaS services, as well as mission-critical banking workloads. An application team request as many subscriptions as they want into the various landing zones, and will be responsible for development, testing, and production. From a platform perspective, each subscription is treated equally (i.e., everything is treated as production) to reduce friction in deployment, operations, and overall continuous compliance.

- **Cloud-Native:** This is the dedicated Management Group for cloud-native landing zones, meaning workloads that may require direct internet inbound/outbound connectivity and will not require connectivity to on-premises. Access to these applications is done either via the internet or via Azure Private Link.

- **Corp:** This is the dedicated Management Group for Corp landing zones, meaning workloads that requires connectivity/hybrid connectivity with the corporate network through the hub VNet in the connectivity subscription.

- **Playground:** This is the dedicated Management Group for subscriptions that will solely be used for testing and exploration by anyone in the organization that want to explore Azure services. Further, subscriptions placed in this management group can be leveraged to expedite service enablement for an FSI organization, to assess and validate Azure services with best practices such as Microsoft Cloud Security Benchmark before allowing new services to be deployed to landing zone subscriptions.

>Note: Playground will be securely isolated and disconnected from everything else within the tenant by Azure policies, be subject to an budget and cost enforcement, and is not meant to provide a path from test, dev, or production.

- **Deprecated:** This is the dedicated Management Group for landing zones that are being cancelled, which then will be moved to this Management Group before deleted by Azure after 30-60 days. Policies will ensure no further resource deployment or writes can occur on these subscriptions.

## Deployment instructions

This section will describe how to deploy FSI Landing Zones reference implementation with traditional "hub and spoke" networking topology in Azure connected to the Distributed Edge and to on-premises datacenters and branch offices.

### Pre-requisites

Since FSI Landing Zones is a complete, end-to-end setup of your Azure tenant as a whole, the user making the deployment requires the "Owner" permission at the Azure tenant root scope. The following instructions explains how a Global Admin in the Azure Active Directory can elevate himself/herself - or others to have the required permissions prior to starting the deployment.

>Note: Both the role assignment as well as the deployment is a one-off exercise, and post deployment you can remove the role assignment from the tenant root scope in Azure.

Pre-requisites for deploying FSI Landing Zones on Microsoft Azure:

- Dedicated Azure subscriptions for the platform
  - Management subscription is always required, where Log Analytics and security configuration will be placed, where other deployment options have a strong dependency on this subscription.
  - Connectivity subscription is required if network topology and hybrid connectivity must be created at the same time.
  - Identity subscription is only required if you plan to move your domain controllers to Azure to facilitate for VM migration and workload authentication/authorization within the landing zones.
- Optionally, Azure subscriptions for the landing zones (corp/online). Note; if these subscriptions does not exist during the deployment, you can always create/move subscriptions into the corp/online management groups later and they will conform to the policies in place and be ready for workload deployments.
- A user that is Global Admin in the Azure Active Directory where FSI Landing Zones will be deployed.
- Elevation of privileges of a Global Admin that will grant him/her the "User Access Administrator" role assignment at the tenant root scope.
- An explicit role assignment (Azure RBAC) made at the tenant root scope via Azure CLI or Azure PowerShell (Note: There's currently no graphical user interface to make this role assignment).

#### Elevate Access to manage Azure resources in Azure Active Directory

>Note: The pre-requisites described here is a *one off* exercise. Once the deployment has completed, permission to the tenant root ("/") can safely be removed as the user will have Owner permission at the intermediate root management group (below tenant root ("/") and tenant root management group).

1.1 Sign into the Azure portal as the user being Global Admin.

1.2 Open Azure Active Directory.

1.3 Under 'Manage', select 'Properties'.

1.4 Under 'Access management for Azure resources' set the toggle to *Yes.

![properties](../docs/properties.png)

Grant explicit access to the User at the tenant root scope ("/") to deploy FSI Landing Zones

You can use either Bash (Azure CLI) o1r PowerShell (Az.Resources) to create the role assignment for user that will do the deployment.

>Note: It is not required to deploy the FSI Landing Zones on Microsoft Azure as a global admin, as the role assignment in next step can be assigned to a different user/group.

Bash:

```bash
#sign into AZ CLI, this will redirect you to a web browser for authentication, if required
az login

#assign Owner role to Tenant root scope ("/") as a Owner (gets object Id of the current user (az login))
az role assignment create --scope '/' --role 'Owner' --assignee-object-id $(az ad signed-in-user show --query "objectId" --output tsv)
```

Powershell:

```powershell
#sign in to Azure from Powershell, this will redirect you to a web browser for authentication, if required
Connect-AzAccount

#get object Id of the current user (that is used above)
$user = Get-AzADUser -UserPrincipalName (Get-AzContext).Account

#assign Owner role to Tenant root scope ("/") as a User Access Administrator
New-AzRoleAssignment -Scope '/' -RoleDefinitionName 'Owner' -ObjectId $user.Id
```

> Please note: it can take up to 15 minutes for permission to propagate at tenant root scope. It is therefore recommended that you log out and log back in to refresh the token before you proceed with the deployment.*

## Step by step deployment guidance

This section will explain the deployment experience and the options provided for FSI Landing Zones reference implementation.

Once the pre-requisites have been completed, you can deploy the FSI Landing Zones using this link [*Deploy to Microsoft Cloud*](https://aka.ms/fsilz), it will start the deployment experience in the Azure portal into your default Azure tenant. In case you have access to multiple tenants, ensure you are selecting the right one.

### 1 - Deployment location

On the first page, select the *Region*. This region will primarily be used to place the deployment resources in an Azure region, but also used as the initial region for some of the resources that are deployed, such as Azure Monitoring resources.

### 2 - Management Group & Subscription Organization

Provide a prefix that will be used to create the management group hierarchy and platform resources, and select if you would use dedicated subscriptions or a single subscription for platform resources (please note that dedicates subscriptions are recommended, and single platform subscription is only for PoC and testing purposes). For this scenario, select **Dedicated**.

![Tenant setup](../docs/tenantsetup.png)

### 3 - Management and Monitoring

Provide a dedicated subscription that will be placed in the management *management group*. In order to enable the rich security, compliance, and logging capabilities within the platform, a Log Analytics workspace is required. You can select the retention days, and optionally configure data export of critical security logs to Event Hub to be collected by third-party SIEM tools.

![Monitoring](../docs/monitoring.png)

Optionally, select the Azure Monitor solutions you want to be enabled for the Log Analytics workspace.

![Solutions](../docs/solutions.png)

### 3 - Security, Governance, and Compliance

On the *Security, Governance, and Compliance* tab, you will configure the core security and policies for your overall platform and landing zones. The FSI Landing Zones will be using the *Microsoft Cloud Security Benchmark* which will be a policy assignment at the intermediate root management group, used for all-up compliance reporting. This is the only policy that will be assigned that contains audit - or auditIfNotExists effect.

In order to enforce a secure by-default posture for the tenant, we provide a list of "secure by-default" Azure services that are fully governed using Azure Policy with deny, deployIfNotExists, and modify effect. These policies will be assigned at the appropriate scopes to ensure continious compliance for the platform and the workload.

![Compliance](../docs/compliance.png)

>Note: When you select an Azure Service in the secure by-default drop-down list, once selected, relevant policies will be assigned, and the resourceTypes related to the service will be added explicitly to the allow list of resources that can be created into the landing zones, into the initial region you are deploying into - and/or enabling as part of the *Network Connectivity and Topology* tab during deployment.

![Compliant Azure services](../docs/compliantservices.png)

Microsoft Defender for Cloud can protect and secure your cloud resources, and you can specify what to enable per each of the supported services.
All the Defender for Cloud settings will be enforced using Azure Policy on every subscription.

![Defender for Cloud](../docs/defender.png)

![Defender services](../docs/defenderservices.png)

For an Azure native cloud SIEM solution, you can also enable Microsoft Sentinel which will be created on the Log Analytics workspace defined in the previous tab, for Management and Monitoring.

![Sentinel](../docs/sentinel.png)

### 4 - Network Connectivity and Topology

On the *Network Connectivity and Topology* tab, you will configure the core networking platform resources, such as hub virtual network, gateways (VPN and/or ExpressRoute), Azure Firewall, Private DNS Zones for Azure PaaS services, Private DNS resolver, and depending on the options you selected on the *Security, Governance, and Compliance* tab, additional networking security and monitoring options will be presented, such as DDoS Protection Standard, Network Watcher, NSG Flow Logs, and Traffic Analytics.

To deploy and configure a network topology, you must:

- Select either "Hub and spoke (customer managed)" or "Virtual VWAN (Microsoft managed). In this guidance we will select the "Hub and spoke".
- Provide a dedicated (empty) subscription that will be used to host the requisite networking infrastructure.
- Provide the address space to be assigned to the hub virtual network.
- Select an Azure region where the hub virtual network will be created. It is recommended that you select the exact same Azure region as you used on the first step in the deployment, where you selected the region for the overall deployment.
- Depending on the Azure region, you can optionally use Azure Virtual Network Manager to manage your virtual networks at scale. For this guide we will not deploy Azure Virtual Network Manager.

 ![img](../docs/hubspoke.png)

In addition, you can configure:

- If you want to deploy Azure Private DNS Zones for accessing Azure PaaS services via Private Link (for example, privatelink.blob.core.windows.net). Select Yes if you want these Private DNS Zones to be deployed.
- Azure DNS Private Resolver can also be deployed in the connectivity hub VNet. Select **Yes** if you want this Azure resource to be deployed in the connectivity hub. If you choose to deploy it, you will have to provide CIDR ranges for the two required subnets (inbound and outbound endpoints).

 ![img](../docs/dns-resolver-and-zones.png)

- VPN and ExpressRoute Gateways. If you choose to deploy either or both of these gateways, you will have the option to select the subnet to be dedicated for these resources, decide if you want to deploy them as regional or zone-redundant gateways, as well as choose the right SKU based on your requirements.

 ![img](../docs/vpn-er-gws.png)

If you choose to deploy Azure Firewall in the hub VNet, you will have the option to
- Select the subnet within the hub VNet
- Select to deploy Azure Firewall as regional or zone redundant (recommended)
- Select the Firewall SKU (Standard or Premium). It is recommended to choose the Azure Firewall [Premium](https://docs.microsoft.com/azure/firewall/premium-features) SKU if your organization requires next generation firewall capabilities such as TLS inspection or network intrusion detection and prevention system (IDPS).
- Indicate if you want to enable DNS Proxy in Azure Firewall (this option is only available if Azure DNS Private Resolver is not deployed).

![img](../docs/azfw.png)

- The deployment experience allows you to deploy internet inbound and outbound on different subscriptions if that is your preference. If that's the case, the portal experience will allow you to specify the subscriptions for internet inbound and outbound, as well as specify whether you want to deploy Azure Firewall on them or not.

![img](../docs/internet-inbound-outbound-subscriptions.png)

For Networking security and monitoring solutions:

- DDoS Protection Standard to be enabled for the Azure platform. If you select **Yes**, all virtual networks (platform and landing zones) will be protected with a DDoS Protection plan.
- Network Watcher observability for the virtual networks that will be created. Enabling this will assign Azure Policy to ensure that Network Watcher will be created into all subscriptions containing a virtual network.
- NSG Flow Logs can be enabled for all Network Security Groups, and will be stored into a storage account in the dedicated subscription for *Management and Monitoring* subscription, and also in the Azure Log Analytics workspace.
- Traffic Analytics which provides a curated view of networking traffic flows.

 ![img](../docs/nwlogs.png)

### 5 - Identity and Access

On the *Identity and Access* tab you can specify if you want to assign recommended policies to primarily secure and govern domain controllers in Azure, which will have its dedicated subscription to ensure clear separation of concerns, and to provide AuthN/AuthZ to workloads into the landing zones. You can then select which policies you want to get assigned, and you will need to provide the address space for the virtual network that will be deployed on this subscription. Please note that this virtual network will be connected to the hub virtual network via VNet peering.

If you don't need - or plan to host domain controllers in Azure for your FSI  workloads, you can select *No*. If you later want to add dedicated subscription for these purposes, you can move it manually to the 'identity' management group in the hierarchy created by FSI Landing Zones.

 ![img](../docs/identity.png)

### 6 - Playground

 The Playground tab allows you to move subscriptions into the playground management group. The intention of playground is to provide subscriptions where developers and business units can experiment with Azure services in a safe and secure way, completely separated from the landing zones (production). Playground is also a good starting point for any service enablement processes within the FSI organization, to assess an Azure service from a security baseline perspective. Additionally, policies can be assigned during the setup that prevents any accidental configuration that can reach any other Azure subscription within the architecture, provides a budget baseline to control cost and spending, as well as recommended policy to prevent usage of the more expensive SKUs for Azure services.

 ![Playground](../docs/playground.png)

### 7 - Landing Zones

On the *Landing Zones* tab, you can bring in the subscriptions you want to use initially for *cloud-native*, and *corp* landing zones. Each landing zone type is represented by its own child management group of the *landing zones* management group in the hierarchy, and provides different characteristics regarding networking requirements, security, policy, and availability.

Depending on your configuration on the *Network Connectivity and Topology* tab, you can optionally add corp-connected landing zones and create virtual networks that will be peered with the connectivity hub created earlier. Optionally, you can select to place subscriptions into corp and cloud-native, and Azure Policy will bring the subscriptions into their target compliance state during the deployment.

![Landing zones](../docs/landingzones.png)

### Review + create

*Review + Create* page will validate your permission and configuration before you can click deploy. Once it has been validated successfully, you can click *Create*
