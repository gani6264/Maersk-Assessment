Q1 - SCENARIO
A car rental company called FastCarz has a .net Web Application and Web API which are recently
migrated from on-premise system to Azure cloud using Azure Web App Service
and Web API Service.
The on-premises system had 3 environments Dev, QA and Prod.
The code repository was maintained in TFS and moved to Azure GIT now. The TFS has daily builds which
triggers every night which build the solution and copy the build package to drop folder.
deployments were done to the respective environment manually. The customer is planning to setup
Azure DevOps Pipeline service for below requirements:

1) The build should trigger as soon as anyone in the dev team checks in code to master branch.
Answer:  while creating new build pipeline, select source and then configure poject name, Repository and Default branch. Here Default branch should be the master branch

2) There will be test projects which will create and maintained in the solution along the Web and API.
The trigger should build all the 3 projects - Web, API and test.
The build should not be successful if any test fails.
Answer: In Build pipeline, have to provide build solution file path (**\*.sln) in build solution task and In Test Assemblies task need to provide test files path 
**\$(BuildConfiguration)\*test*.dll
!**\obj\**

For test fails, need to uncheck the Continue on error in Test Assemblies task in build pipeline

3) The deployment of code and artifacts should be automated to Dev environment.
Answer: once build completed all artifacts are store in drop location. In release pipeline need to configure build artifact pipeline name and enable Continuous deployment trigger. In Dev env, Pre deployment conditions triggers should be select after release

4) Upon successful deployment to the Dev environment, deployment should be easily promoted to QA
and Prod through automated process.
Answer: In QA env, Pre-deployment conditions triggers should be after stage same like prod as well 

5) The deployments to QA and Prod should be enabled with Approvals from approvers only.
Answer: Need to configure pre deployment approvals and post deployment approvals 



Q2 - SCENARIO
Macro Life, a healthcare company has recently setup the entire Network and Infrastructure on Azure.
The infrastructure has different components such as Virtual N/W, Subnets, NIC, IPs, NSG etc.
The IT team currently has developed PowerShell scripts to deploy each component where all the
properties of each resource is set using PowerShell commands.
The business has realized that the PowerShell scripts are growing over period of time and difficult to
handover when new admin onboards in the IT.
The IT team has now decided to move to Terraform based deployment of all resources to Azure.
All the passwords are stored in a Azure Service known as key Vault. The deployments needs to be
automated using Azure DevOps using IaC(Infrastructure as Code).

Here  I am going to provision the infra using ARM templates.

1) What are different artifacts you need to create - name of the artifacts and its purpose
   In azure devops repository need to commit the vm.json and vm.parameters.json files. onec we have those 2 json files in repo, will start run build pipeline but in build pipeline need to add copy file task and then publish artifacts. In both tasks target folder sholud be same ($(build.artifactstagingdirectory)). will create .zip
2) List the tools you will to create and store the Terraform templates.
   Visual studio code will use to crate and update the arm template
3) Explain the process and steps to create automated deployment pipeline.
   will add ARM template deployment task in release pipeline and configure vm.json file and then vm.parameters.json files
4) Create a sample Terraform template you will use to deploy Below services:
Vnet
2 Subnet
NSG to open port 80 and 443
1 Window VM in each subnet
1 Storage account
5) Explain how will you access the password stored in Key Vault and use it as Admin Password in the VM
Terraform template.

Add Azure keyvault task in release pipeline and in ARM template deployment task configure in override template parameters like -adminPassword $(adminPassword)