Deploy Cypienta User Interface
==============================

Prerequisite
------------

Make sure that you have the required permissions for resources for the IAM user you will be using.

-  ECS
-  EC2
-  IAM - create and edit roles
-  CloudFormation

Deploy using Cloud Formation template
-------------------------------------

1.  Clone the Github repo 

    .. code-block:: shell

        $ git clone -b v0.4 https://github.com/cypienta/Lambda.git
    
    .. note::
        This command will clone the repository and checkout the branch ``v0.4``

2.  Navigate to the AWS console, and search for ``CloudFormation``.

3.  Click on ``Stacks`` on the left hand side panel, and click on ``Create stack`` dropdown. Select ``With new resources (standard)`` to start creating a stack

    .. image:: resources/create_stack_start.png
        :alt: create stack
        :align: center

4.  For the ``Prerequisite - Prepare template`` section, select ``Choose an existing template``, and then select ``Upload a template file``. It will enable a ``Choose file`` button. Click on the button to upload the template. The template ``ui-template.yaml`` is present in the root directory of Lambda repository you have cloned. Then click on ``Next``.

    .. image:: resources/upload_template_file.png
        :alt: upload template
        :align: center

5.  Now you can input all the parameters needed for the cloud formation stack. Few parameters are already filled in with default recommended value. You can change the values as required.
    
    Give a name to the stack in ``Stack name``.

    Fill in the following parameter values as they require user input:

    **SuperuserEmail:**\ The email for admin user for UI

    **SuperuserUsername:**\ The username of the admin user for UI

    **SuperuserPassword:**\ The password of the admin user for UI

    **WebContainerImage:**\ The container image of the subscribed marketplace UI product with tag ``market*``. The ``Web container image`` noted in the section :doc:`subscribe`.

    **NginxContainerImage:**\ The container image of the subscribed marketplace UI product with tag ``nginx-market*``. The ``Nginx container image`` noted in the section :doc:`subscribe`.

    The constraints for choosing the ``Cpu`` and ``Memory`` for the cluster can be found `here <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ecs-taskdefinition.html#cfn-ecs-taskdefinition-cpu>`__

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

You may now go to the step :doc:`start_using` to start testing
your application.
