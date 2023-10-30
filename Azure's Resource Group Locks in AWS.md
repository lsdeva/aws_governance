For large-scale organizations that want to implement a mechanism similar to Azure's Resource Group Locks in AWS, the most relevant and commonly used approaches are:

AWS Organizations with Service Control Policies (SCPs):

Scope: SCPs allow organizations to define permission boundaries for member accounts. These policies can restrict or allow actions on specific AWS resources or services across multiple accounts.
Objective: To ensure that accounts within an organization conform to certain permission boundaries, preventing unwanted actions on resources.
AWS CloudFormation Stack Policies:

Scope: Directly applied to CloudFormation stacks. When an organization uses CloudFormation to provision resources, stack policies can prevent specific resources within the stack from being updated or deleted.
Objective: Protect specific resources within a CloudFormation deployment from unintentional changes, especially during stack updates.
AWS Service Catalog:

Scope: Allows organizations to define and manage approved AWS resources. The Service Catalog can restrict the provisioning and modification of AWS resources to predefined configurations.
Objective: Enforce standardization, governance, and compliance across resource deployments by allowing only predefined and approved configurations.
