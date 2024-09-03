Configure Elastic
=================

Prerequisites
-------------

Make sure that you have deployed the Cypienta application detailed in :doc:`../getting_started/deploy` before integrating.

Logstash pipeline from Elastic Search to AWS S3
-----------------------------------------------

Follow the steps below to add a Logstash integration and configure it to
move events from Elastic Search to AWS S3.

Add Logstash Integration
~~~~~~~~~~~~~~~~~~~~~~~~

1. Click on the search bar on the top of the page of your Elastic
   console and search for ``Logstash``. Click on the ``Logstash``
   integration.

    .. image:: elastic_resources/search_logstash.png
        :alt: Search logstash
        :align: center

2. To add your first integration, click on ``Install Elastic Agent``

    .. image:: elastic_resources/install_agent.png
        :alt: Install agent
        :align: center

3. Follow the steps as shown on the page ``Install Elastic Agent``. If
   using linux tar, copy the commands from the ``Linux Tar`` tab, and
   paste it on the machine on which you want to install elastic agent.

    .. image:: elastic_resources/download_tar.png
        :alt: Download tar
        :align: center

   Note: Elastic Agent will be installed at ``/opt/Elastic/Agent``


    You will get a confirmation when the agent enrollment is confirmed:

    .. image:: elastic_resources/agent_confirmed.png
        :alt: Agent confirmed
        :align: center

4. Click on ``Add the integration`` button, and then click on ``Confirm
   incoming data``

    .. image:: elastic_resources/confirm_incoming_data.png
        :alt: Agent confirmed
        :align: center

5. Now on the machine where the elastic agent is installed, install
   logstash. `Refer <https://www.elastic.co/guide/en/logstash/current/installing-logstash.html>`__ the documentation for the same.

Create Role for configuring Logstash
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. To create a Role. Open the left panel and expand ``Management``, and
   click on ``Stack Management``

    .. image:: elastic_resources/stack_management.png
        :alt: Stack Management
        :align: center

2. Again, on the left panel, find the section ``Security``, and click on
   ``Roles``. On the ``Roles`` page, click on ``Create role`` button.

3. Referring to the
   `authorization <https://www.elastic.co/guide/en/logstash/current/plugins-inputs-elasticsearch.html#plugins-inputs-elasticsearch-autz>`__
   required for the role used by the ``elasticsearch`` input plugin. Give
   a name to the role you want to create and add the following
   privileges:

   **Cluster privileges:**\ Select ``monitor`` privilege.

   **Index privileges:**\ Under ``Indexes``, select the index you want to
   give permission to. (Assuming that an Index is already created). Add
   ``read`` privilege to corresponding to the index.

   Click on ``Create role`` to create the role.

    .. image:: elastic_resources/create_role.png
        :alt: Create role
        :align: center

Create User for configuring Logstash
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. To create a User. Open the left panel and expand ``Management``, and
   click on ``Stack Management``

    .. image:: elastic_resources/stack_management.png
        :alt: Stack Management
        :align: center

2. Again, on the left panel, find the section ``Security``, and click on
   ``Users``. On the ``Users`` page, click on ``Create user`` button.
3. Add necessary details to create User

    .. image:: elastic_resources/create_user.png
        :alt: Create user
        :align: center

4. Under privileges section. Select the role that was created in
   previous step.

    .. image:: elastic_resources/select_role.png
        :alt: Select role
        :align: center

5. Click on ``Create user`` to create the user. Keep note of the
   ``Username``, and ``Password`` for the User.

Configure Logstash
~~~~~~~~~~~~~~~~~~

Once the installation of Logstash is complete. Follow the instructions
below to configure Logstash.

1. Install necessary plugins for Logstash. Required input plugin is
   `elasticsearch <https://www.elastic.co/guide/en/logstash/current/plugins-inputs-elasticsearch.html>`__,
   and output plugin is
   `s3 <https://www.elastic.co/guide/en/logstash/current/plugins-outputs-s3.html>`__

   Navigate to Logstash installation directory:

    .. code-block:: shell

        $ cd /usr/share/logstash
        

   Install the required plugins:

    .. code-block:: shell

        $ sudo bin/logstash-plugin install logstash-input-elasticsearch 
        $ sudo bin/logstash-plugin install logstash-output-s3

2. Create configuration file

    .. code-block:: shell

        $ sudo nano /etc/logstash/conf.d/logstash.conf

3. Add the following configuration to the configuration file created. Replace ``your_index_name`` with the index of the alerts generated from a rule.

    .. code-block:: 

        input {
            elasticsearch {
                hosts => "http://elastic-localhost:9200" # Replace with your Elasticsearch host
                index => "your_index_name" # Replace with your index name
                user => "your_username" # Replace with your Elasticsearch username created
                password => "your_password" # Replace with your Elasticsearch password
                schedule => "* * * * *" # Schedule to run every minute
                size => 500 # Number of documents to fetch per run
                scroll => "5m" # Scroll context time
                docinfo => true
            }
            }

            output {
            s3 {
                access_key_id => "your_access_key_id" # Replace with your AWS Access Key ID
                secret_access_key => "your_secret_access_key" # Replace with your AWS Secret Access Key
                region => "your_region" # Replace with your AWS region, e.g., "us-east-1"
                bucket => "your_bucket_name" # Replace with your S3 bucket name
                prefix => "your_folder_prefix/" # Optional, specify the folder prefix in the bucket
                time_file => 5 # Number of minutes before creating a new file in S3
                size_file => 10485760 # Size in bytes before creating a new file in S3 (10MB)
                codec => "json_lines" # Format of the output file
            }
        }

    To get the index of the alerts for a rule. You may open an alert details, and click on ``JSON`` tab. The field value ``_index`` is the index of the alert.

    .. image:: alert_index.png
        :alt: get index
        :align: center


Note: For more information on the elasticsearch input plugin, click
`here <https://www.elastic.co/guide/en/logstash/current/plugins-inputs-elasticsearch.html>`__.
For more information on the s3 output plugin click
`here <https://www.elastic.co/guide/en/logstash/current/plugins-outputs-s3.html>`__

Start Logstash
~~~~~~~~~~~~~~

Start Logstash with the configuration file you created.

.. code-block:: shell

    $ sudo systemctl start logstash

Check the status to ensure Logstash is running correctly

.. code-block:: shell

    $ sudo systemctl status logstash

Optional Steps:
^^^^^^^^^^^^^^^

**Monitor Logstash Logs:**\ Logs can be found in the
``/var/log/logstash/`` directory. Use these logs to troubleshoot any
issues that may arise.

**Test Logstash Configuration:**

Check Logstash Configuration Syntax. The following command will check
the configuration file for syntax errors.

.. code-block:: shell

    $ sudo /usr/share/logstash/bin/logstash --config.test_and_exit -f /etc/logstash/conf.d/logstash.conf

If there are no errors, you will see a message indicating that the
configuration is OK.

Verify Data Flow
~~~~~~~~~~~~~~~~

1. Run Logstash in the Foreground

   Run Logstash in the foreground to observe its behavior and debug any
   issues. This will also allow you to see the logs in real-time.

    .. code-block:: shell

        $ sudo /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash.conf

2. Verify the that data is being written to your specified S3 bucket.
   You should see files being created in the bucket, following the
   configuration specified in ``logstash.conf``.

    .. image:: elastic_resources/verify_data_transfer.png
        :alt: Select role
        :align: center