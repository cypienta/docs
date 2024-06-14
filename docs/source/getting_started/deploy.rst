AWS Deployment
==============

Deploy resources using the Cloud Formation template
---------------------------------------------------

1. Clone the Github repo 

    .. code-block:: shell

        $ git clone -b v0.4 https://github.com/cypienta/Lambda.git
    
    .. note::
        This command will clone the repository and checkout the branch ``v0.4``

2. Navigate to the AWS console, and search for ``CloudFormation``.

3. Click on ``Stacks`` on the left hand side panel, and click on ``Create stack`` dropdown. Select ``With new resources (standard)`` to start creating a stack

    .. image:: resources/create_stack_start.png
        :alt: Subscribe to technique detector
        :align: center

4. For the ``Prerequisite - Prepare template`` section, select ``Choose an existing template``, and then select ``Upload a template file``. It will enable a ``Choose file`` button. Click on the button to upload the template. The template is present in the root directory of Lambda repository you have cloned. Then click on ``Next``.

    .. image:: resources/upload_template_file.png
        :alt: Subscribe to technique detector
        :align: center

5. Now you can input all the parameters needed for the cloud formation stack. Few parameters are already filled in with default recommended value. You can change the values as required.
    
    Give a name to the stack in ``Stack name``.


    Fill in the following parameter values as they require user input:

    **BucketName:**\ The name of S3 bucket that you want to create.
    (required to change as the current value populated may not be
    valid). Follow these
    `rules <https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html#general-purpose-bucket-names>`__
    for naming a bucket. Constraint of the bucket name by AWS is that
    the bucket name must be globally unique. So note that your cloud
    formation stack may fail if the name provided is already taken. You
    can see the failure reasons by clicking on the stack that was
    created and clicking on the ``Events`` tab.

    **TechniqueModelARN:**\ The ARN of the subscribed model package for
    ATTACK Technique detector

    **ClusterModelARN:**\ The ARN of the subscribed model package for
    Temporal Clustering

    **FlowModelARN:**\ The ARN of the subscribed model package for MITRE
    flow detector

    **SuperuserEmail:**\ The email for admin user for UI

    **SuperuserUsername:**\ The username of the admin user for UI

    **SuperuserPassword:**\ The password of the admin user for UI

    **WebContainerImage:**\ The container image of the subscribed marketplace UI product with tag ``market*``. The ``Web container image`` noted in the section :doc:`subscribe`.

    **NginxContainerImage:**\ The container image of the subscribed marketplace UI product with tag ``nginx-market*``. The ``Nginx container image`` noted in the section :doc:`subscribe`.

    The constraints for choosing the ``Cpu`` and ``Memory`` for the cluster can be found `here <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ecs-taskdefinition.html#cfn-ecs-taskdefinition-cpu>`__

    Recommended value for parameter:

    **ChunkSize:**\ The size of a single chunk that will be processed at a
    time for an input file uploaded to S3. Recommended chunk_size is
    below ``50000``.

6.  Click on ``Next`` after adding the parameters.

7.  On the page ``Configure stack options``, under the section ``Stack
    failure options``, select ``Roll back all stack resources`` for
    ``Behaviour on provisioning failure``. Select ``Delete all newly
    created resources`` for ``Delete newly created resources during a
    rollback``. And then click on ``Next``.

8.  Now in the ``Review and create`` page, you can review your parameters.
    At the bottom of the page, select all checkboxes for ``I
    acknowledge…`` and click on ``Submit``. This will start creating the
    required resources.

9.  You can monitor the events of the cloud stack by clicking on the
    recently created cloud stack and going to the ``Events`` tab.

10. Once the cloud stack is completed successfully. You can start using
    the products.

Now all your resources are ready to be used.

You may now go to the step :doc:`end_to_end_test` to start testing
your application.
