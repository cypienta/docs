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

How to delete stack
-------------------

1. Navigate to the AWS console and search for ``CloudWatch``. Make sure you are in the same region in which you created CloudFormation stack.

2. On the left hand side panel, under ``Logs``, click on ``Log groups``. Select all the check boxes for the ``Log groups`` that were created by the CloudFormation stack, click on ``Actions`` dropdown and click on ``Delete log group(s)``, and then click on ``Delete`` button.

3. Next, search for ``S3`` in the AWS console search bar.

4. Select the bucket that was created from the CloudFormation stack and click on ``Empty``. Type in ``permanently delete`` in the confirmation box and click on ``Empty``.

5. Now search for ``CloudFormation`` in the the AWS console search bar.

6. Open the stack that you want to delete and click on ``Delete``. Wait for the entire stack to be deleted before you move on to creating new stack.

    .. note::
        If there are any failures in deleting the stack, then ``Retry delete``.
        
        To speed up delete for stack, follow the optional steps below:

        1. Navigate to AWS console and search for ``ECS`` and select ``Elastic Container Service``.
        
        2. Click on the ECS cluster deployed from the stack. Select all the service from the ``Services`` tab and click on ``Delete service``. Check the box for ``Force delete`` and type in ``delete`` in the confirmation box and then click on ``Delete``.

        3. Navigate to AWS console and search for ``EC2``.

        4. Manually delete the running EC2 instance with name ``* - <ECS-cluster-name>``. Select all the pertinent instances, click on the ``Instance state`` dropdown and click on ``Terminate instance``.

Common Mistakes
----------------

Some of the common errors that can result in the failure of the CloudFormation stack:

- Duplicate S3 bucket name
- Incorrect arn for Models/UI
- Incorrect Image for VRL Lambda 

Duplicate S3 bucket name
~~~~~~~~~~~~~~~~~~~~~~~~

In the case of a duplicate S3 bucket name, delete the failed CloudFormation stack,
then choose a new globally unique S3 bucket name and recreate the stack.

Incorrect arn for Models/UI
~~~~~~~~~~~~~~~~~~~~~~~~

In the case of an incorrect arn for models/UI, delete the failed CloudFormation stack,
then confirm the arns for all models and UI components as seen in :doc:`subscription` and recreate the stack.

Incorrect Image for VRL Lambda 
~~~~~~~~~~~~~~~~~~~~~~~~

In the case of an incorrect image for VRL lambda, delete the failed CloudFormation stack,
then ensure that you have the correct ECR Image URI and version number, and recreate the stack. 

Common errors
-------------

CapacityError: Unable to provision requested ML compute capacity. Please retry using a different ML instance type.
~~~~~~~~~~~~~~~

If the SageMaker batch transform job fails for ``transform-job-cluster-*`` with the error 
``CapacityError: Unable to provision requested ML compute capacity. Please retry using a different ML instance type.`` 
the batch transform job can be retriggered manually. Follow the steps below to retrigger:

1. Open the lambda function ``create_cluster``.

2. Click on the ``Configuration`` tab, then click on ``Environment variables``. 
Click on ``Edit`` button, and click on ``Add environment variable``. Under the ``Key`` text field enter ``batch_transform_job_suffix``, under ``Value`` text field enter any unique value. Limit the text value to length of 3. For example, ``1``. And, click on ``Save`` button.

3. Open the S3 bucket created by the CloudFormation stack. Navigate to ``scratch/output/classification/<unique_id>/``. 

4. Select the ``input.json``, click on ``Actions``, click on ``Copy``. On the Copy page, click on ``Browse S3``, click on ``Choose destination``, and then click on ``Copy``.

5. This will trigger a new batch transform job.

If the SageMaker batch transform job fails for ``transform-job-flow-*`` with the error 
``CapacityError: Unable to provision requested ML compute capacity. Please retry using a different ML instance type.`` 
the batch transform job can be retriggered manually. Follow the steps below to retrigger:

1. Open the lambda function ``create_flow``.

2. Click on the ``Configuration`` tab, then click on ``Environment variables``. 
Click on ``Edit`` button, and click on ``Add environment variable``. Under the ``Key`` text field enter ``batch_transform_job_suffix``, under ``Value`` text field enter any unique value. Limit the text value to length of 3. For example, ``1``. And, click on ``Save`` button.

3. Open the S3 bucket created by the CloudFormation stack. Navigate to ``scratch/output/cluster/<unique_id>/``. 

4. Select the ``input_flow.json``, click on ``Actions``, click on ``Copy``. On the Copy page, click on ``Browse S3``, click on ``Choose destination``, and then click on ``Copy``.

5. This will trigger a new batch transform job.
