# PI6-objectives

Write for PI-6 objectives Jira tickets.

## PR1: Security Inventory Automation (PI-6 Epic)

As a security analyst, there is a need to understand the libraries, components, and vulnerabilities running in AWS and see the results in a single view. The solution(s) should use cloud-native tooling wherever possible. If the capability can be delivered using AWS services, the they are preferred over any other external solution.

Use Case:
log4j is vulnerable, which of our assets have it?

Acceptance Criteria:
single-view inventory of libraries, components and vulnerabilities

Research potentional tooling in AWS for delivering this ask.
Inspector, SecurityHub, Config, SSM, OpsCenter, ECR

Explore external tooling to understand non AWS offerings:
DataDog, Qualys (EC2), Crowdstrike Falcon (containers), MuleSoft ?? (libraries, components, vulnerability management), Sonatype (vulnerabilities)

Implementation Task(s)

1. Get Overview of AWS services with Amazon (PR1 Implementation Task)

Consult with AWS to get an overview, and potentially a deeper dive of AWS services that provide cataloging capability. Attend the AWS Enterprise Support Office Hours for CSBS that occurs every other week on Tuesdays at 3 to 4 PM ot setup a special session with Jeremy Thomas (thojrm@amazon.com) and Kevin Shaw (awsshaw@amazon.com). Find out what AWS services work together to understand the libraries, components, and vulnerabilities running in AWS and see the results in a single view and how to buid that solution.

AWS Services:
Inspector, SecurityHub, Config, SSM, OpsCenter, ECR

Acceptance Criteria:
A strawman design of how these services would provide a single view and what that view would be. 

2. Consult with Outside Vendors (PR1 Implementation Task)
Even if AWS cloud-native services does meet the need, consult with outside vendors to see how their tools can be used for prevention or mitigation. This implemention task should embelish the 
strawman design using AWS services by covering a process for mitigation and prevention or if they play no role at all.

Tools/Vendors:
Sonatype IQ Server/Nexus Pro - Sonatype
Qaulys - Qualy Inc
DataDog - Datadog
Crowdstrike Falcon - Crowdstrike
Mulesoft? - Mulesoft

3 Document a proposed Security Inventory Automation design and implementation (PR1 Implementation Task)
Create a proposed design, implemention, and process. This would be an architecture document or PPT presentation.

Acceptance Criteria:
Set up a review with Solution Architects, @Jason Miller and other stakeholders. All stakeholders sign off on the proposed solution.

4 Implement approved solution (POC) (PR1 Implementation Task)

Implement the design. This may be a POC type activity but if it works it should be quickly switched to production solution. If a new AWS accounts are required, work with Forerunners who are working on Control Tower. It may be potentially necessary to break this task down futher implementation inder this Epic once the details of the solution are better understood.

Acceptance Criteria:
single-view inventory of libraries, components and vulnerabilities.

------------------------

## PR2: SSL Certificate Automation (PI-6 Epic)

As a cloud architect and security analyst, there is a need to automate the creation and renewal of certificates in AWS an other internal resources. Currently, certifiates are bought from GoDaddy by someone at CSBS with a credit card and imported into Amazon Certificate Manager (ACM). Additionally, there is a wild card certicate that is used on various load balancers across accounts. When these cerificates need to renew there is alway a scramble to cover them all.

There is a desire to automate this process and monitor expiration so certificate renewal can be planned inside a well defined maintnance window. For certificates created and managed by Amazon, they are automated rotated and renewed. For certificates used outside of Amazon this not the case and some source other than GoDaddy should be pursued and an automation "tool" would be created to renew certificates (Letsencrypt.org). Location of this auto mation is still to be determined.

With EKS workloads, certficates will be created in ACM as part of the deployment using Terraform https://registry.terraform.io/modules/terraform-aws-modules/acm/aws/latest. So they are covered. As deployments are moved away from CDK to Terraform, the same module can be used even if the deployment is not EKS. For certificates provisioned outside of ACM, they should be imported into ACM using Terraform and their status monitored. The current thinking is Datadog can serve that purpose.

Since the Amazon side of this is automated, the work in this epic is largely focused on certificates coming from an external source. Some of this may be dependant on the aviability of Datadog for monitoring.

Acceptance criteria: certificates are automatically provisioned with objects that require SSL endpoints and renewed automatically with no human effort. Internal resources that cannot use ACM should use a form of automation in an understood and blessed maintenance window. 

1 Investigate a certificate monitoring with Datadog (PR2 Implementation Task)

Datadog has the ability to monitor TLS. This includes the monitoring certificate expiration. To understand this better, work with Datadog subject matter experts to see if certificate alerts are available when certain thresholds are met. Since the automation is operting in a predefined maintenanace window, the automation would first reach out the source for info.  

If Datadog is not available, then other monitoring tools should be pursued or one can use openssl. The use of open SSL would require more scripting to parse the output. Then there is the issue from where it woudld run.

Accptance Criteria: Integration points with Datadog or monitoring solution well understood and documented.

2. Propose a certifcate automation design and process flow. (PR2 Implementation Task)

Create an automation design and process flow describing how the automation would work. This would be an architecture document or PPT presenation. This would need to identify where the automation would run, define what maintenance window to use, desribe how it would integrate to a monitoring source,a dn how the implemenation wil be tested 

Acceptance Criteria:
Set up a review with Solution Architects, @Jason Miller and other stakeholders. All stakeholders sign off on the proposed solution.

3 Implement proposed certificate automation solution. (PR2 Implementation Task)

Implement the proposed solution provided in step 2. Applu a set of test criteria prefiousy defined.

Acceptance Criteria:
Demonstrate certificates can automatically renew expiring certificates wihtout huma intervention in a test bed.

## PR3: Define and Create a Production Ready EKS Cluster

As an intastructure engineer, there is a desire to begin to deploy application on AWS Elastic Kubernetes Service (EKS) clusters. More and more vender offerings are being delivered with Kubernetes implementation that are fully packaged with Hem, highly configurable, and easier to deploy and maintina.

There are a small number of EKS cluster already exist within CSBS AWS and more are coming. Before EKS clusters grow profilerate, it is important to define an architecture for EKS that defines a base definition of what a production ready cluster entails. Whether following a pattern of cluster per application deployed as a single unit or cluster as a service ready for susequent appliction deploymnt, this archtecture server starting poing upon which applications are deployed.

The work under this Epic begins with an architectural definition that will have input across Audience Crickets adn Forerunners team. The implemention that follows will also be shared across teams in close collaboration so everyn plays apart in the enterprise solution. The implementation will leverage Terraform modules to create the cluster and will be built in iteration following th architectural blueprint.

The architecture needs to touch multiple aspects of the cluster supported by Terraform modules:
Networking
  VPC
  Subnets
IAM Roles
Cluster
  Networking
    VPC 
    subnets
    seconary CIDRS
  API Server Endpoint
  Security Groups
  Metrics
    Metrics Server
    Sluster Autoscaling & Tags
  Compute
    Managed Node Groups
      AMI
      EC2
      Node Affinity
    Fargate
  Load Balancer Controller
  Monitoring (Datadog)
  Network Policies
  Secret Management (Front Runners)
  Logging (splunk or is that there automatically?)
  Helm Chart Deployments
  Helm Chart Ownership
Applications
  Certificates
  DNS
  Service Accounts for IAM Roles
  AD & Okta Integrtion
CI/CD
  Integrated deployment with GitLab CI/CD (Forerunners)
Billing

Acceptance Criteria:
An AWS EKS architectural design to follow and a Terraformm Stack that deploys an EKS cluster. Note, some solutions nay not be supported directly by Terraform and require Kubernetes and Helm or Deploy test applications to exercise the various features of the cluster.

Compute - Fargate vs managed nodes (likely managed nodes, Fargate is expensive) 
Modules 
Metrics server
Networking 
Endpoints public or private? 
Public or private subnets? 
Certificates 
Datadog agents 
Tagging subnets 
Autoscaling 
Tagging autoscaling groups 
Sidecar tools 
Network policies 
Secrets management (in conjunction with FR) 
Helm chart ownership 
Helm chart deployments (from inside the same repo or from an external repo?) 
Use Forward Architecture document 
Savings plan

1 Create an AWS EKS Architecture Document (PI-6 Implementation Task 5 points)
Create an EKS Architeure document at a shared location for collaboration and comments across AC and Forerunners teams. The architecture should identify design decisions for each of the following:

Networking
  VPC
  Subnets
IAM Roles
Cluster
  Networking
    VPC 
    subnets
    seconary CIDRS
  API Server Endpoint
  Security Groups
  Metrics
    Metrics Server
    Sluster Autoscaling & Tags
  Compute
    Managed Node Groups
      AMI
      EC2
      Node Affinity
    Fargate
  Load Balancer Controller
  Monitoring (Datadog)
  Network Policies
  Secret Management (Front Runners)
  Logging (splunk or is that there automatically?)
  Helm Chart Deployments
  Helm Chart Ownership
Applications
  Certificates
  DNS
  Service Accounts for IAM Roles
  AD & Okta Integrtion
CI/CD
  Integrated deployment with GitLab CI/CD (Forerunners)
Billing

2 Define least prviliged AWS EKS Role for cluster creation

3 Create a Base Cluster and Managed Node Group (PI-6 Implementation Task 2 points)

Create a base cluster with public endpoint, default AMIs, three private subnets, three t3.small managed nodes in the sandbox account. Terrform state will be stored in S3

Acceptance Criteria:
Attach to the cluster context and
2 Add Metrics Server and Cluster Autoscaler
3 Add Load Balancer Controller
4 Add Network Policy Support
5 Add Secret Management
6 Add Monitoring Agents (Datadog)
7 API Server Endpoint Public/Private Solution
8 Select and apply an application example to demostrate HPA
9 Select and apply an application example to demostrate cluster autoscaling
10 Select and apply an application example to demostrate network policy
11 Select and apply an application to demonrate secrets management
12 Select and apply an application to demonstrate monitoring & logging (datadog)
13 Select and apply an application to demonstrate DNS and certificate management
14 Placeholder for CI/CD integration and Terraform State Management (Forerunners)



https://github.com/sonatype/helm3-charts
https://artifacthub.io/packages/helm/sonatype/nexus-iq-server
https://help.sonatype.com/repomanager3/product-information/system-requirements
