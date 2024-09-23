Prerequisites
=============

Permissions
-----------
Make sure that you have the required permissions for resources for the IAM user you will be using.

-  Lambda
-  S3
-  ECS
-  EC2
-  ECR
-  IAM
-  CloudFormation
-  Step Functions

To confirm you have the required permssion for the resources necessary to run the 
pipeline you can check that with the following script. To run the script the iam user must have ``iam:SimulatePrincipalPolicy`` policy.

.. code-block:: console

    $ wget -O- https://raw.githubusercontent.com/cypienta/AWS/v0.8/check_permissions.py | python 

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


Lambda concurrent executions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Verify that quota limit for ``Concurrent executions`` for AWS Lambda function. On your AWS console for the region where you want to deploy your resources, Search for ``Service Quotas``, and select ``AWS Lambda`` from the AWS Services list. Search for quota name ``Concurrent executions``. Make sure that the applied account-level quota value is more than 12 to allow reserved concurrency for the enrich_with_technique, update_lookup_table lambda function. If the value is not greater than 10, select the ``Concurrent executions`` and click on ``Request increase at account level`` and set to any value greater than 10.

