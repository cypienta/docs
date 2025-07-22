Prerequisites
=============

Permissions
-----------
Make sure that you have the required permissions for resources for the IAM user you will be using.

-  S3
-  ECS
-  EC2
-  ECR
-  EFS
-  IAM
-  CloudFormation
-  Lambda
-  EventBridge
-  EventBridge Scheduler

To confirm you have the required permssion for the resources necessary to run the 
pipeline you can check that with the following script. To run the script the IAM user must have ``iam:SimulatePrincipalPolicy``, ``iam:GetUser`` and ``iam:GetUserPolicy`` policy.

.. code-block:: console

    $ wget -O- https://raw.githubusercontent.com/cypienta/AWS/v0.10.0/check_permissions.py | python


Quotas
------

Instance types
~~~~~~~~~~~~~~

Verify your instance type quotas by going to the AWS console. Search for ``Service Quotas``, and select ``Amazon Elastic Compute Cloud (Amazon EC2)`` from the AWS Services list. Search for ``Running On-Demand Standard (A, C, D, H, I, M, R, T, Z) instances``. Request for an increase of quota if found to be less than 35. The EKS will be automatically adding and removing this instance type nodes to the cluster as needed.

.. note::
    Example: 
        - Given the target region, go to service quotas or visit https://us-east-2.console.aws.amazon.com/servicequotas/home/services/sagemaker/quotas
        - Search and select "Running On-Demand Standard (A, C, D, H, I, M, R, T, Z) instances" or visit https://us-east-1.console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-1216C47A
            - If the applied account-level quota value is less than 35, request an increase to at least 35.


VPC and Internet Gateways
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Verify that there are enough quota limit for creating 1 VPC, 1 Internet Gateway, 2 public subnets for 1 template deployment. On your AWS console for the region where you want to deploy your resources, Search for ``Service Quotas``, and select ``Amazon Virtual Private Cloud (Amazon VPC)`` from the AWS Services list. Search for relevant quota names and make sure that the applied account-level quota value is as desired. There should be enough quota to create at least 1 VPC, 1 Internet Gateway, and 2 public subnets.
