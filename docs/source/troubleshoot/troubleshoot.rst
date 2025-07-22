Troubleshoot
========


How to delete stack
-------------------

1. Search for ``CloudFormation`` in the the AWS console search bar.

2. Open the stack that you want to delete and click on ``Delete``.

3. Next, search for ``S3`` in the AWS console search bar.

4. Select the bucket that was created from the CloudFormation stack and click on ``Empty``. Type in ``permanently delete`` in the confirmation box and click on ``Empty``.

    .. note::
        If you need to backup the data in the bucket, you can download the data from the bucket you want to keep and then empty the bucket. The data once deleted cannot be recovered.


Airflow Errors
--------------

1.  Go to the Cypienta UI and login with your credentials.

    .. image:: resources/ui_login.png
        :alt: Login to UI
        :align: center


    .. note::
        The default credentials are present in :doc:`start_using` page.

2.  On the left hand side panel, expand ``Settings`` and click on ``Pipeline``, then click on the ``Errors`` tab.

    .. image:: resources/bastet_airflow.png
        :alt: Airflow
        :align: center

3. The tab ``Error`` shows the Airflow Error List that will show the errors that have occurred in the Airflow pipeline.

4. Click on the ``Go to Airflow`` link at the top right to go to the Airflow UI.

.. 5. If you want to rerun the task, follow the steps below in section :ref:`Rerun Airflow task <rerun_task>`.


.. Common errors
.. -------------

.. .. _rerun_task:

.. Airflow Task failure
.. ~~~~~~~~~~~~~~~~~~~

.. If any DAG run in airflow has failed, then you may manually rerun the task. Follow the steps below to rerun the task:

.. 1. Login to the Airflow UI.

.. 2. Navigate to the DAG that has failed.

.. 3. On the left hand side panel which shows all the DAG runs, select the failed DAG task.

..     .. image:: resources/failed_dag_task.png
..         :alt: failed_dag_task
..         :align: center

.. 4. Click on the ``Clear task`` button on the top right corner of the page.

..     .. image:: resources/failed_task_clear_task.png
..         :alt: failed_task_clear_task
..         :align: center

.. 5. Click on the ``Clear`` button to clear the task.

..     .. image:: resources/failed_task_clear.png
..         :alt: failed_task_clear
..         :align: center


S3 schema
---------

Folder structure
~~~~~~~~~~~~~~~~

The S3 bucket folder structure is as follows:

.. code-block:: text

    bucket/
    ├── input/
    │   └── Common_Event_Format/
    │       └── input.json
    ├── clustering_agent/
    |   ├── config_1.json
    │   └── config_2.json
    ├── output/
    │   └── 2024-08-08 21:22:52 +0000/
    |       ├── cluster.json
    |       ├── event.json
    │       └── flow.json
    ├── mapping/
    │   └── field_mapping/
    │       └── Common_Event_Format.json
    ├── uploads/
    ├── output/
    └── scratch/

**input/:** The input folder contains all the files that will be processed by the Cypienta pipeline. Once the file is created in this folder, the file is added to the queue to be processed in a step function execution. There will be one step function execution per file in the input folder in sequential order. The status of the current execution can be viewed on Airflow UI.

**output/:** The output folder contains event, cluster, flow output for the input that was processed by the Cypienta pipeline.

**scratch/:** The folder contains necessary files that are required for proper functioning of the Cypienta pipeline.

**clustering_agent/:** The folder contains the configuration files for the clustering agent to be used for the clustering model.

**mapping/:** The folder contains the mapping files for the Cypienta pipeline.

**uploads/:** The folder contains the files that are uploaded to the S3 bucket from the UI.

**output/:** The folder contains the output files that are created by the Cypienta pipeline.