Support
=======


If you need help with Cypienta, please send us an emailat support@cypienta.com with the following details:

-  Your contact information
-  The steps you took before getting blocked by an error
-  The error message you received

Error in Cypienta UI
--------------------

If the issue is related to the Cypienta UI, please include the following details.

-  The user actions performed before the error
-  The screenshot of the error message as seen on the UI
-  The Cloudformation stack name you are using the Cypienta UI from
-  The timestamp of the error occurrence: This can be noted down from the system you using to access the UI. It does not need to be exact but within a window of 5 minutes should be fine.
-  (Optional) Please see the :ref:`Getting logs from CloudWatch <getting_logs_from_cloudwatch>` section below for more details on how to get the logs from the CloudWatch logs


.. _getting_logs_from_cloudwatch:

Getting logs from CloudWatch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Navigate to the AWS console and search for CloudWatch.
2. On the left hand side panel, expand the ``Logs`` and click on ``Log Groups``.
3. Search for the log group with the name ``/ecs/<stack-name>-cluster``.
4. Click on the log group and then click on ``Log Streams``, this will list all the log streams for the stack.
5. In the ``Log Streams`` list, search for ``bastet_web``. As by default the log streams are sorted by last event time, the most recent log stream will be at the top.
6. You may search for error messages by clicking on the ``Filter`` button and then typing ``error`` or ``exception`` or ``warning``.
7. If you find any relevant logs, please copy them and paste them in the support email.
8. Repeat the steps 6, 7 for the ``bastet_celery`` and ``bastet_celery_beat`` log streams.


Error in pipeline
-----------------

If the issue is related to the pipeline. Please follow the steps below:

1. Navigate to the Airflow UI. This can be done by clicking on the ``Airflow`` button in the Cypienta UI. OR by getting the Load Balancer URL from the Cloudformation stack ``Outputs`` tab and navigating to it.

2. Once inside the Airflow UI, navigate to the DAG that is ``failing``. This could be seen easily from the red color circle on the DAG runs.

    .. image:: resources/airflow_failed_dag.png
        :alt: Airflow failed DAG
        :align: center

3. Click on the failed DAG run with red color circle. This will open a new page with list of all the DAG runs that have failed.

    .. image:: resources/airflow_failed_dag_runs.png
        :alt: Airflow failed DAG runs
        :align: center

4. Note down the following things from the page:

    -  DAG ID
    -  Run ID
    -  Conf

5. (Optional) Please see the :ref:`Getting logs from Airflow <getting_logs_from_airflow>` section below for more details on how to get the logs from the Airflow logs


.. _getting_logs_from_airflow:

Getting logs from Airflow
~~~~~~~~~~~~~~~~~~~~~~~~~

1. To get the logs for a specific task, click on the ``Run ID`` link. This will open a new page with run details for the selected DAG run. In that pertinent column, look for a task that is in ``failed`` state (red in color). Click on any one of the failed task, either on the left hand side panel or on the ``Graph`` tab.

    .. image:: resources/failed_dag_logs.png
        :alt: Airflow failed task logs
        :align: center

2. This will open up a context for that particular task. Click on the ``Log`` tab to view the logs for that task.

    .. image:: resources/failed_task_logs.png
        :alt: Airflow failed task logs
        :align: center

3. Copy the logs and paste them in the support email. You may mask any sensitive information if any.
