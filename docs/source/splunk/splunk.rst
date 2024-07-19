Configure Splunk
================

Getting AWS Access key
----------------------

To get data from and to S3, the Apps for Splunk would require Access keys from AWS. Follow the steps below to get Access key. If you already have Access key and corresponding Secret key, you can skip to :ref:`getting_data_from_splunk` 

1. Navigate to AWS console and search for ``IAM``.

2. On the left hand side panel, under ``Access Management``, select ``Users``

    .. image:: splunk_resources/iam_users_panel.png
        :alt: select users from panel
        :align: center

3. Click on the user for whom you want to create Access key. Select the tab ``Security credentials`` and find ``Access keys`` section. Click on ``Create access key`` button on top right of the section.

    .. image:: splunk_resources/access_key_tab.png
        :alt: select users from panel
        :align: center

4. On the ``Access key best practices & alternatives`` page, select ``Other`` and click on ``Next``.

    .. image:: splunk_resources/other_access_key.png
        :alt: select other
        :align: center

5. Set an optional description tag for the access key and click on ``Create access key``.

6. Make note of the ``Access key`` and ``Secret access key`` to use in later steps. You may also download .csv file by clicking on ``Download .csv file``.

    .. image:: splunk_resources/copy_access_key.png
        :alt: copy access key
        :align: center


.. _getting_data_from_splunk:

Getting data from Splunk to S3
------------------------------

To get search results of Splunk to AWS S3. Follow the steps below:

1. Login to the splunk instance. Click on the ``Apps`` drop down from the top panel. Select ``Find More Apps``

    .. image:: splunk_resources/find_apps.png
        :alt: Find more apps
        :align: center

2. Search for ``Amazon S3 Uploader``, and find the ``Amazon S3 Uploader for Splunk`` app from the list. Click on ``Install``, and enter your credentials to install the app.

    .. note::
        More details for the Amazon S3 Uploader for Splunk app can be found `here <https://apps.splunk.com/app/6958/#/details>`__

3. After installing the app, move to the home page, and click on ``Apps`` again. You should now see ``Amazon S3 Uploader for Splunk`` in the list. Click on the app and a configuration page will appear.

    .. image:: splunk_resources/s3_app.png
        :alt: Select S3 app
        :align: center

4. On the configuration page. Click on the ``Account`` tab, and click on ``Add`` to add an AWS account.

    In the ``Logging`` tab, the ``Log level`` is set to ``INFO`` by default, modify it as required.

    .. image:: splunk_resources/app_config.png
        :alt: Configure app
        :align: center

5. Now move to the search tab, and write a query

    .. image:: splunk_resources/search_tab.png
        :alt: Search for events
        :align: center

6. Verify that you have received the desired events. And then click on the ``Save As`` button on top of the search bar, and select ``Alert``.

    .. image:: splunk_resources/save_alert.png
        :alt: Save query as alert
        :align: center

7. Next, add the ``Title`` and ``Description`` for the alert, setup alert schedule and trigger conditions as required. And under the Trigger Actions section, click on ``Add Actions`` button. Select ``Upload to Amazon S3`` option.

    .. image:: splunk_resources/select_action.png
        :alt: Configure action for alert
        :align: center

8. Add the ``Bucket name`` which was created using the CloudFormation template to save the results. For ``Object key``, enter ``splunk_input/input/%d-%b-%Y %H:%M:%S.json``. Select ``Account`` that you created on the configuration page from the dropdown. Finally click ``Save``. 

    .. note::
        The user-provided object key is passed to Python's ``datetime.strftime()`` function, which encodes the time the search started. Format codes are extremely similar to Splunk's, please refer to the `official documentation <https://docs.python.org/3.7/library/datetime.html#strftime-strptime-behavior>`__.

        To debug logs from the app, search with the query ``index=_internal source="/opt/splunk/var/log/splunk/amazon_s3_upload_modalert.log"``. The default logging level is ``INFO``, but it can be increased or decreased from the configuration dashboard.


Getting data from S3 to Splunk
------------------------------

To get results of cypienta product from S3 to Splunk. Follow the steps below:

1. Login to the splunk instance. Click on the ``Apps`` drop down from the top panel. Select ``Find More Apps``

    .. image:: splunk_resources/find_apps.png
        :alt: Find more apps
        :align: center

2. Search for ``S3``, and find the ``Splunk Add-on for Amazon Web Services (AWS)`` app from the list. Click on ``Install``, and enter your credentials to install the app.

    .. note::
        More details for the Splunk Add-on for Amazon Web Services (AWS) app can be found `here <https://apps.splunk.com/app/1876/#/overview>`__

3. After installing the app, move to the home page, and click on ``Apps`` again. You should now see ``Splunk Add-on for AWS`` in the list. Click on the app and click on the ``Configuration`` tab to get configuration page for the app.

    .. image:: splunk_resources/splunk_aws_app.png
        :alt: Select S3 app
        :align: center

4. On the configuration page. Click on the ``Account`` tab, and click on ``Add`` to add an AWS account.

    In the ``Logging`` tab, the ``Log level`` is set to ``INFO`` by default, modify it as required.

    .. image:: splunk_resources/splunk_add_on_conf_tab.png
        :alt: Configure app
        :align: center

5. Now move to the ``Inputs`` tab. Click on ``Create New Input`` button, select ``S3 Access Logs``, then select ``Incremental S3``.

    .. image:: splunk_resources/incremental_s3.png
        :alt: Search for events
        :align: center

6. On the ``Add Incremental S3`` page, give a name to the configuration. Select the ``AWS Account`` that was created in the previous step. Select the ``S3 Bucket`` which was created using the CloudFormation template, and provide the ``Log File Prefix`` of ``splunk/``. Under ``Splunk-related Configuration`` configure the ``Log Start Date`` and ``Index`` of your choice and click on ``Add``.

    .. image:: splunk_resources/conf_input.png
        :alt: configure input
        :align: center

7. Now click on the search tab, and write a query

    .. image:: splunk_resources/s3_to_splunk_search.png
        :alt: Search for events
        :align: center


Configure integration with JIRA
-------------------------------

Integrate the JIRA management to Splunk SOAR to create event for each JIRA issue created.

1. Install JIRA add-on app for Splunk SOAR. Go to the ``Apps`` page on splunk SOAR.

    .. image:: splunk_resources/select_add_on.png
        :alt: Configure action for alert
        :align: center

2. Click on the ``New Apps`` button and then search for ``jira``. There will be a result for ``JIRA``, appearing for the add-on app. Click on ``Install`` button to install the add-on app.

3. To configure the app, click on ``Configure New Asset``
    
    .. image:: splunk_resources/get_add_on.png
        :alt: Configure action for alert
        :align: center

    Initially the app will be listed under ``Unconfigured apps``.

4. In ``Asset name`` field, add a name of your choice.

    .. image:: splunk_resources/configure_asset.png
        :alt: Configure action for alert
        :align: center

5. Move to ``Asset Settings`` tab. Give a JIRA URL, username, API token and project key from which you want to poll and sync Splunk SOAR events from. 
    Select the ``Maximum tickets (issue) to poll first time`` as a number greater than the total number of JIRA issues present in the JIRA management at the time of configuring the add-on.
    Select the ``Maximum ticket (issues) for scheduling polling`` as a number of latest issues that you want to poll each time.

    .. image:: splunk_resources/asset_setting.png
        :alt: Configure action for alert
        :align: center

6. Move to ``Ingest Settings`` tab. For the ``Label to apply to objects from this source`` field, select ``events`` and set the ``Select a polling interval or schedule to configure polling on this asset`` to ``Interval``. Select polling interval ``Polling interval (minutes)`` of your choice.

    .. image:: splunk_resources/ingest_setting.png
        :alt: Configure action for alert
        :align: center

7. Click on ``Save`` button to save the config for the add-on. Wait for the interval minutes set to allow Splunk SOAR to start polling JIRA issues to Splunk SOAR events.

8. Use the ``poll now`` button to poll the JIRA issues right now. Set the ``Maximum containers`` as the same value as set for ``Maximum tickets (issue) to poll first time``. Set ``maximum artifacts`` to a desired value, and click on ``Poll Now`` button.

    .. image:: splunk_resources/poll_now.png
        :alt: Configure action for alert
        :align: center
