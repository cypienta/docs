Prerequisites
=============

Permissions
-----------
Make sure that you have the required permissions for resources for the IAM user you will be using.

-  S3
-  ECS
-  EC2
-  ECR
-  IAM
-  CloudFormation
-  Lambda

To confirm you have the required permssion for the resources necessary to run the 
pipeline you can check that with the following script. To run the script the iam user must have ``iam:SimulatePrincipalPolicy`` policy.

.. code-block:: console

    $ wget -O- https://raw.githubusercontent.com/cypienta/AWS/v0.9/check_permissions.py | python 

Quotas
------

Instance types
~~~~~~~~~~~~~~

Verify your instance type quotas by going to the AWS console. Search for ``Service Quotas``, and select ``Amazon Elastic Compute Cloud (Amazon EC2)`` from the AWS Services list. Search for ``Running On-Demand G and VT instances`` or ``Running On-Demand P instances``. You will require a GPU instance type for ``ATTACK Technique Detector`` and ``Temporal Clustering``, so look at the supported and recommended instance types for the product before subscribing and request for an increase of quota if found to be less than 1. The recommended GPU instance types are g4dn and p2. The ``MITRE ATTACK Flow Detector`` requires a CPU-based instance type such as c5.

.. note::
    Example: 
        - Given the target region, go to service quotas or visit https://us-east-2.console.aws.amazon.com/servicequotas/home/services/sagemaker/quotas
        - Search and select "Running On-Demand G and VT instances" or visit https://us-east-2.console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-DB2E81BA
            - If the applied account-level quota value is less than 1, request an increase to at least 1.
    
    If you want to select a p3 instance. Follow the steps below:
        - Search and select "Running On-Demand P instances" or visit https://us-east-2.console.aws.amazon.com/servicequotas/home/services/ec2/quotas/L-417A185B
            - If the applied account-level quota value is less than 1, request an increase to at least 1.


VPC and Internet Gateways
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Verify that there are enough quota limit for creating one VPC, one Internet Gateway, two public subnets for one template deployment. On your AWS console for the region where you want to deploy your resources, Search for ``Service Quotas``, and select ``Amazon Virtual Private Cloud (Amazon VPC)`` from the AWS Services list. Search for relevant quota names and make sure that the applied account-level quota value is as desired.
