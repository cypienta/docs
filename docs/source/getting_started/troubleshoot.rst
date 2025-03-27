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

5. In the EC2 AWS Service, navigate to ``Volumes`` under ``Elastic Block Storage`` and select all the volumes with name ``<ECS-cluster-name>_*``. And click on ``Actions`` and then click on ``Delete volume``. If the ``Volume state`` is not in ``Available`` state, then you will have to wait for the cloud formation stack to complete the deletion.

    .. note::
        You need to do the deletion of the volumes manually as the cloud formation stack does not complete the deletion of the volumes as they are persistent volumes. If you do not delete the volumes, and create a new stack with the same as the one you deleted earlier in the same region, you will not start from a clean slate and may face issues.


Delete stack failed
~~~~~~~~~~~~~~~~~~~

In case there is a failure while deleting the stack, follow the steps below to manually delete few blocking stack resources:

1. Search for ``CloudFormation`` in the the AWS console search bar.

2. Open the stack that you want to delete and click on ``Delete``.

3. Navigate to AWS console and search for ``ECS`` and select ``Elastic Container Service``.

4. Click on the ECS cluster deployed from the stack. Check that there are no services running. If there are any, select all the service from the ``Services`` tab and click on ``Delete service``. Check the box for ``Force delete`` and type in ``delete`` in the confirmation box and then click on ``Delete``.

5. Click on the ``Tasks`` tab and select all the tasks and click on ``Stop task``.

6. Navigate to AWS console and search for ``EC2``.

7. Manually reduce the desired capacity of the Auto Scaling Groups with name ``<ECS-cluster-name>-*``. Select each auto scaling group and select ``Actions`` dropdown and select ``Edit``. Reduce the ``Desired capacity`` to ``0`` and reduce the ``Min desired capacity`` to ``0``. Click on ``Update``.

8. Manually delete the running EC2 instance with name ``* - <ECS-cluster-name>``. Select all the pertinent instances, click on the ``Instance state`` dropdown and click on ``Terminate instance``.


Common errors
-------------

Airflow Task failure
~~~~~~~~~~~~~~~~~~~

If any DAG run in airflow has failed, then you may manually rerun the task. Follow the steps below to rerun the task:

1. Login to the Airflow UI.

2. Navigate to the DAG that has failed.

3. On the left hand side panel which shows all the DAG runs, select the failed DAG task.

    .. image:: resources/failed_dag_task.png
        :alt: failed_dag_task
        :align: center

4. Click on the ``Clear task`` button on the top right corner of the page.

    .. image:: resources/failed_task_clear_task.png
        :alt: failed_task_clear_task
        :align: center

5. Click on the ``Clear`` button to clear the task.

    .. image:: resources/failed_task_clear.png
        :alt: failed_task_clear
        :align: center


S3 schema
---------

Folder structure
~~~~~~~~~~~~~~~~

The S3 bucket folder structure is as follows:

.. code-block:: text

    bucket/
    ├── input/
    │   └── cypienta_cef/
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
    │       └── cypienta_cef.json
    ├── uploads/
    └── scratch/

**input/:** The input folder contains all the files that will be processed by the Cypienta pipeline. Once the file is created in this folder, the file is added to the queue to be processed in a step function execution. There will be one step function execution per file in the input folder in sequential order. The status of the current execution can be viewed on Airflow UI.

**output/:** The output folder contains event, cluster, flow output for the input that was processed by the Cypienta pipeline.

**scratch/:** The folder contains necessary files that are required for proper functioning of the Cypienta pipeline.

**clustering_agent/:** The folder contains the configuration files for the clustering agent to be used for the clustering model.

**mapping/:** The folder contains the mapping files for the Cypienta pipeline.

**uploads/:** The folder contains the files that are uploaded to the S3 bucket from the UI.