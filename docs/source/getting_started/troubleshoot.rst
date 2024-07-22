Troubleshoot
========

ERROR: ResourceLimitExceeded for batch transform job instance type.
-------------------------------------------------------------------

Increase the quota for the account using Service Quotas. Navigate to the
AWS console for the region you are using. Search for ``Service Quotas``

1. Search for “Sagemaker” under ``Manage quotas`` and select ``Amazon
   SageMaker`` AWS service. Click on view quotas.

    .. image:: resources/service_quotas.png
        :alt: Service Quota AWS console
        :align: center

2. Search for ``<instance-type> for transform job usage``

    .. image:: resources/quota_instance_types.png
        :alt: Quoatas for Instance types
        :align: center

3. Select the required instance type from the list and click on ``Request
   increase at account level``.

Common Mistakes
----------------

Some of the common errors that can result in the failure of the CloudFormation stack:

- Duplicate S3 bucket name
- Incorrect arn for Models/UI
- Incorrect Image for VRL Lambda 

Duplicate S3 bucket name
--------------------------
In the case of a duplicate S3 bucket name, delete the failed CloudFormation stack,
then choose a new globally unique S3 bucket name and recreate the stack.

Incorrect arn for Models/UI
----------------------------
In thge case of an incorrect arn for models/UI, delete the failed CloudFormation stack,
then confirm the arns for all models and UI components as seen in :doc:`subscription` and recreate the stack.

Incorrect Image for VRL Lambda 
-------------------------------
In the case of an incorrect image for VRL lambda, delete the failed CloudFormation stack,
then ensure that you have the correct ECR Image URI and version number, and recreate the stack. 