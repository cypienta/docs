AWS Deployment
==============

.. _setup_lambda_repository_single_command:

Setup Lambda repository - Single quick command
-----------------------

1. Navigate to the AWS console, and select ``CloudShell`` at the bottom left of the console. Open the cloud shell in the region you want to deploy.

2. The following command will download shell script to setup lambda repository and output an ECR Image URI. Make note of the ECR Image URI to use in CloudFormation template.

    .. code-block:: shell

        $ wget https://github.com/cypienta/AWS/raw/v0.7/vrl-lambda.sh && sh vrl-lambda.sh
    
    .. note::
        Move to the section :ref:`setup_lambda_repository` for manual detailed steps.

3. Once you make note of the ECR image URI for the VRL lambda, move to the section :ref:`deploy_cloud_formation`

.. _setup_lambda_repository:

Setup Lambda repository - Detailed manual steps
-----------------------

1. Navigate to the AWS console, and select ``CloudShell`` at the bottom left of the console. Open the cloud shell in the region you want to deploy. You may run the following one line command, or continue to next step to run individual commands.

    .. code-block:: shell

        $ export AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query "Account" --output text) && export REPO_NAME="cypienta-vrl-lambda" && docker pull public.ecr.aws/p2d2x2s3/cypienta/vrl-lambda:v0.1 && aws ecr create-repository --repository-name ${REPO_NAME} && export ECR_URI="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com" && aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_URI} && docker tag public.ecr.aws/p2d2x2s3/cypienta/vrl-lambda:v0.1 ${ECR_URI}/${REPO_NAME}:v0.1 && docker push ${ECR_URI}/${REPO_NAME}:v0.1 && echo ${ECR_URI}/${REPO_NAME}:v0.1

2. Store the AWS Account ID, and ECR repository name to environment variable in cloud shell.

    .. code-block:: shell

        # Save AWS Account ID as environment variable
        $ export AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query "Account" --output text)

        # Replace value with ECR repository name you want to give
        $ export REPO_NAME="cypienta-vrl-lambda"

3. Pull the Cypienta VRL Lambda image from the AWS public repository using the following command.

    .. code-block:: shell

        $ docker pull public.ecr.aws/p2d2x2s3/cypienta/vrl-lambda:v0.1

4. Once the image pull is completed, create an ECR repository to push the Cypienta VRL Lambda image.

    .. code-block:: shell

        $ aws ecr create-repository --repository-name ${REPO_NAME}

5. After successfully create ECR repository, you can navigate to ECR private repository to view the responsitory you just created.

    .. image:: resources/lambda_ecr.png
        :alt: lambda ecr repo
        :align: center

6. Run the following commands to push the image to ECR repository.
    
    .. code-block:: shell

        # Create ECR URI for ECR repository
        $ export ECR_URI="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
        # Login to the ECR repository
        $ aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_URI}
        # Tag pulled image to push to ECR repository
        $ docker tag public.ecr.aws/p2d2x2s3/cypienta/vrl-lambda:v0.1 ${ECR_URI}/${REPO_NAME}:v0.1
        # Push the image to ECR repository
        $ docker push ${ECR_URI}/${REPO_NAME}:v0.1

7. Copy the ECR Image URI and make a note of it to use in CloudFormation template

    .. code-block:: shell

        $ echo ${ECR_URI}/${REPO_NAME}:v0.1

8. Once you make note of the ECR image URI for the VRL lambda, move to the section :ref:`deploy_cloud_formation`

.. _deploy_cloud_formation:

Deploy resources using the Cloud Formation template
---------------------------------------------------

1. On your local machine, download the template file from Github. `Template file <https://github.com/cypienta/AWS/blob/862fe7a6a28a3be7c8f3367d142d5464a2f52037/template.yaml>`__. Or, use the following command to download the ``template.yaml`` file.

    .. code-block:: shell

        $ wget https://github.com/cypienta/AWS/raw/v0.7/template.yaml
    
    .. note::
        Run this command on your local machine. This command will download the template.yaml file.

2. Navigate to the AWS console, and search for ``CloudFormation``.

    .. note::
        The UI component deployed from this template is only supported in the following AWS Regions. Make sure that you create stack in the supported region.
        Supported AWS regions: eu-north-1, ap-south-1, eu-west-3, us-east-2, eu-west-1, eu-central-1, sa-east-1, ap-east-1, us-east-1, ap-northeast-2, eu-west-2, ap-northeast-1, us-west-2, us-west-1, ap-southeast-1, ap-southeast-2, ca-central-1

3. Click on ``Stacks`` on the left hand side panel, and click on ``Create stack`` dropdown. Select ``With new resources (standard)`` to start creating a stack

    .. image:: resources/create_stack_start.png
        :alt: Subscribe to technique detector
        :align: center

4. For the ``Prerequisite - Prepare template`` section, select ``Choose an existing template``, and then select ``Upload a template file``. It will enable a ``Choose file`` button. Click on the button to upload the template. The template is present in the root directory of Lambda repository you have cloned. Then click on ``Next``.

    .. image:: resources/upload_template_file.png
        :alt: Subscribe to technique detector
        :align: center

5. Now you can input all the parameters needed for the cloud formation stack. A few parameters are already filled in with default recommended values. You can change the values as required.
    
    Give a name to the stack in ``Stack name``.


    Fill in the following parameter values as they require user input:

    **BucketName:** The name of S3 bucket that you want to create.
    (required to change as the current value populated may not be
    valid). Follow these
    `rules <https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html#general-purpose-bucket-names>`__
    for naming a bucket. Constraint of the bucket name by AWS is that
    the bucket name must be globally unique. So note that your cloud
    formation stack may fail if the name provided is already taken. You
    can see the failure reasons by clicking on the stack that was
    created and clicking on the ``Events`` tab.

    **TechniqueModelARN:** The ARN of the subscribed model package for
    ATTACK Technique detector. Use version 0.4 Product ARN for the region in which CloudFormation stack is created.

    **ClusterModelARN:** The ARN of the subscribed model package for
    Temporal Clustering. Use version 0.7 Product ARN for the region in which CloudFormation stack is created.

    **FlowModelARN:** The ARN of the subscribed model package for MITRE
    flow detector. Use version 0.7 Product ARN for the region in which CloudFormation stack is created.

    **SuperuserEmail:** The email for admin user for UI

    **SuperuserUsername:** The username of the admin user for UI

    **SuperuserPassword:** The password of the admin user for UI

    **VRLLambdaImage:** The container image of the VRL Lambda that was pushed to ECR private repository in :ref:`setup_lambda_repository_single_command`

    **WebContainerImage:** The container image of the subscribed marketplace UI product with tag ``market*``. The ``Web container image`` noted in the section :doc:`subscribe`.

    **NginxContainerImage:** The container image of the subscribed marketplace UI product with tag ``nginx-market*``. The ``Nginx container image`` noted in the section :doc:`subscribe`.

    The constraints for choosing the ``Cpu`` and ``Memory`` for the cluster can be found `here <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ecs-taskdefinition.html#cfn-ecs-taskdefinition-cpu>`__

    Recommended value for parameter **ChunkSize** is below ``100000``.

    .. note::
        **ChunkSize:** The size of a single chunk that will be processed at a time for an input file uploaded to S3. 

6.  Click on ``Next`` after adding the parameters.

7.  On the page ``Configure stack options``, under the section ``Stack
    failure options``, select ``Roll back all stack resources`` for
    ``Behaviour on provisioning failure``. Select ``Delete all newly
    created resources`` for ``Delete newly created resources during a
    rollback``. Expand the options for ``Stack creation options - optional`` and under  ``Timeout``, enter ``15`` to set a max timeout of 15 minutes for the stack. And then click on ``Next``.

    .. image:: resources/stack_timeout.png
        :alt: stack timeout
        :align: center

8.  Now in the ``Review and create`` page, you can review your parameters.
    At the bottom of the page, select all checkboxes for ``I
    acknowledge…`` and click on ``Submit``. This will start creating the
    required resources.

9.  You can monitor the events of the cloud stack by clicking on the
    recently created cloud stack and going to the ``Events`` tab.

    .. note::
        **Resource Creation Time:** The cloud stack will take approximately 10 minutes to complete the creation of all the resources. 

10. Once the cloud stack is completed successfully. You can start using
    the products. Click on the ``Outputs`` tab for the recently created cloud 
    stack and note down the load balancer URL for the UI under ``LoadBalancerDNSName``. 
    Click on the link to open the UI.

    .. image:: resources/lb_url.png
        :alt: lb url
        :align: center

Now all your resources are ready to be used.


.. _Handling Multiple Inputs:

Handling Multiple Inputs
-------------------------

    The pipeline will process files in the input folder sequentially in the order of upload.
    Only one file will be processed at a time. Once a file is finished be processed the
    pipeline will start with the next file in the queue automatically.

.. note::
        **Small input files:** For best performance, it is not recommended to upload many
        small files due to the startup time overhead of SageMaker jobs. 
        It is recommended to aggregate small inputs into larger input files.
.. You may now go to the step :doc:`end_to_end_test` to start testing your application.
