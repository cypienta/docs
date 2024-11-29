AWS Deployment
==============

.. _deploy_cloud_formation:

Deploy resources using the Cloud Formation template
---------------------------------------------------

1. On your local machine, download the template file from Github. `Template file <https://github.com/cypienta/AWS/blob/4cdece2a8404bc182eba33637625c2615a725204/template.yaml>`__. Or, use the following command to download the ``template.yaml`` file.

    .. code-block:: shell

        $ wget https://github.com/cypienta/AWS/raw/v0.9/template.yaml
    
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


    All parameter values are pre-filled for quick user experience. Some of the parameters are:

    **SuperuserEmail:** The email for admin user for UI

    **SuperuserUsername:** The username of the admin user for UI

    **SuperuserPassword:** The password of the admin user for UI

    The constraints for choosing the ``Cpu`` and ``Memory`` for the cluster can be found `here <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ecs-taskdefinition.html#cfn-ecs-taskdefinition-cpu>`__

    Recommended value for parameter **ChunkSize** is below ``100000``.

    .. note::
        **ChunkSize:** The size of a single chunk that will be processed at a time for an input file uploaded to S3. 

6.  Click on ``Next`` after adding the parameters.

7.  On the page ``Configure stack options``, under the section ``Stack
    failure options``, select ``Roll back all stack resources`` for
    ``Behaviour on provisioning failure``. Select ``Delete all newly
    created resources`` for ``Delete newly created resources during a
    rollback``. Expand the options for ``Stack creation options - optional`` and under  ``Timeout``, enter ``20`` to set a max timeout of 20 minutes for the stack. And then click on ``Next``.

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
        **Resource Creation Time:** The cloud stack will take approximately 15 minutes to complete the creation of all the resources. 

10. Once the cloud stack is completed successfully. You can start using
    the products. Click on the ``Outputs`` tab for the recently created cloud 
    stack and note down the load balancer URL for the UI under ``CypientaUI``. 
    Load balancer for the airflow will be under ``CypientaAirflow``.
    Bucket name for the S3 bucket will be under ``CypientaBucket``.
    Click on the link to open the UI.

    .. image:: resources/template_output.png
        :alt: lb url
        :align: center

    .. note::
        The default credentials for Cypienta UI: Default ``Username`` is ``cypienta`` and the default ``Password`` is ``cypienta``

        The default credentials for Cypienta Airflow: Default ``Username`` is ``cypienta`` and the default ``Password`` is ``cypienta``

Now all your resources are ready to be used.


Handling Multiple Inputs
-------------------------

The pipeline will process files in the input folder in a batch.
The files will be processed at a scheduled time which can be setup in Cypienta UI. Once a file is finished processing the
pipeline will start with the next batch of files in the queue automatically.

.. note::

    **Handling Large Input Files:** Currently the pipeline can handle upto 100,000 events in single input file. Be mindful of the input file that is used as input.
