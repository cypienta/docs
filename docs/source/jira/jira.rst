Configure JIRA
=================

Prerequisites
-------------

Make sure that you have deployed the Cypienta application detailed in :doc:`../getting_started/deploy` before integrating.

Make sure that you note the JIRA API token, JIRA Username, JIRA URL, and JIRA Project key before setting up the lambda function.

Setup JIRA connection
-----------------------------------------------

Follow the steps below to add a lambda function that will create the jira issue.

Create lambda function
~~~~~~~~~~~~~~~~~~~~~~

1. Navigate to the AWS console in the same region where your Cypienta application is deployed. Search for Lambda in the search bar and click on ``Create function``

2. Select ``Author from scratch``, write the ``Function name`` as ``create_jira`` with desired prefix and suffix. For ``Runtime`` select ``Python 3.11``, and select the ``Architecture`` as ``x86_64``. Expand the ``Change default execution role`` and select ``Use an existing role``. Click on ``Create function`` to create the function.

    .. image:: resources/create_function.png
        :alt: create function
        :align: center

3. Scroll to the bottom of the lambda function that was just created and find the section ``Layers``. Click on ``Add a layer`` to add a layer.

    .. image:: resources/add_layer.png
        :alt: add layer
        :align: center

4. Select ``AWS layers`` as ``Layer source``. Select ``AWSSDKPandas-Python311`` as ``AWS layers`` and select the version that is available in the dropdown. Click on ``Add`` to add the layer to the lambda function.

    .. image:: resources/layer_config.png
        :alt: layer config
        :align: center

5. Now select the ``Configuration`` tab on the lambda function overview page and select ``General configuration`` from the left hand side panel. Click on ``Edit`` button to modify the values.

6. Edit the ``Memory`` field with max value that is available. Edit the ``Ephemeral storage`` field with max value that is available. Edit the ``Timeout`` with max value that is available. And then, click on ``Save``.

    .. image:: resources/general_config.png
        :alt: general config
        :align: center

7. Again on the ``Configuration`` tab on the lambda function overview page, select ``Triggers`` from the left hand side panel. Click on ``Add trigger`` button to add a trigger.

8. For the trigger configuration, select the source as ``S3``. From the bucket dropdown, select the bucket that was created for the Cypienta application. Select ``All object create events`` for ``Event types``. Add Prefix as ``scratch/output/flow/`` and suffix as ``.json``. Check the box to acknowledge the message for recursive invocation and click on ``Add``.

    .. image:: resources/trigger.png
        :alt: add trigger
        :align: center

9. Back on the ``Configuration`` tab, select ``Environment variables`` from the right hand side panel. Click on ``Edit`` to add variables.

10. To add environment vairables, click on ``Add environment variable`` button and enter the following key, value. Finally, click on ``Save``.

    - Key: ``cluster_or_flow``
        Value: ``cluster``
    
    - Key: ``event_threshold``
        Value: ``2``
        Description: The minimum number of events that must be present in a cluster for which a JIRA issue is to be created.

    - Key: ``jira_api_token``
        Value: ``<jira-api-token>``

    - Key: ``jira_lookup_object``
        Value: ``scratch/jira/issues.csv``

    - Key: ``jira_project_key``
        Value: ``<jira-project-key>``

    - Key: ``jira_url``
        Value: ``<jira-url>``
        Sample value: ``https://cypienta-demo.atlassian.net``

    - Key: ``jira_username``
        Value: ``<jira-username>``

11. Get the lambda function `create_jira <https://github.com/cypienta/AWS/blob/577d10bf2f0ae6a12db5bf2cd66207914af89dac/lambda/create_jira.py>`__ and copy paste the code in the ``Code`` tab in the ``Code source`` section. Click on the ``Deploy`` button to save the lambda function.


Now all the new clusters created will be pushed to the JIRA project that was configured in the environment vairables of the lambda function.

Refer to :doc:`../splunk/splunk` for integrating JIRA with Splunk SOAR.
