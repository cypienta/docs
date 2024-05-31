Prerequisites
=============

Permissions
-----------
Make sure that you have the required permissions for resources for the IAM user you will be using.

-  SageMaker
-  Lambda
-  S3
-  ECS
-  EC2
-  IAM - create and edit roles
-  CloudFormation


Quotas
------

Instance types
~~~~~~~~~~~~~~

Verify your instance type quotas by going to the AWS console. Search for ``Service Quotas``, and select SageMaker from the AWS Services list. Search for ``transform job usage``. You will require a GPU instance type for ``ATTACK Technique Detector`` and ``Temporal Clustering``, so look at the supported and recommended instance types for the product before subscribing and request for an increase of quota if found to be less than 1. The ``MITRE ATTACK Flow Detector`` requires a CPU-based instance type.

.. note::
    To check for the supported and recommended instance type. On the AWS marketplace model product page, scroll down to the ``Pricing`` section and click on ``Model Batch Transform`` under ``Software Pricing``.

Lambda concurrent executions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Verify that quota limit for ``Concurrent executions`` for AWS Lambda function. On your AWS console for the region where you want to deploy your resources, Search for ``Service Quotas``, and select ``AWS Lambda`` from the AWS Services list. Search for quota name ``Concurrent executions``. Make sure that the applied account-level quota value is more than 10 to allow reserved concurrency for the update_lookup_table lambda function. If the value is not greater than 10, select the ``Concurrent executions`` and click on ``Request increase at account level`` and set to any value greater than 10.

