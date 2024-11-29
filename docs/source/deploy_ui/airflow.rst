Airflow Configuration and Errors
================================

Airflow Errors
--------------

1.  Go to the Cypienta UI and login with your credentials.

    .. image:: resources/ui_login.png
        :alt: Login to UI
        :align: center


    .. note::
        The default credentials are present in :doc:`start_using` page.

2.  On the left hand side panel, click on ``Airflow``.

    .. image:: resources/bastet_airflow.png
        :alt: Airflow
        :align: center

3. The tab ``Error`` shows the Airflow Error List that will show the errors that have occurred in the Airflow pipeline.

4. Click on the ``Go to Airflow`` link at the top right to go to the Airflow UI.


Airflow Scheduler
-----------------

1.  On the Cypienta UI, on the left hand side panel, click on ``Airflow``.

    .. image:: resources/bastet_airflow.png
        :alt: Airflow
        :align: center

3. Click on the tab ``Schedule``. The default schedule is setup using a cron tab which will trigger the Pipeline every 5 hours.

    .. image:: resources/airflow_schedule.png
        :alt: Airflow
        :align: center

4. To edit the pipeline schedule, click on the ``Edit`` button. The schedule can be edited using the cron tab format.

    .. image:: resources/airflow_schedule_edit.png
        :alt: Airflow
        :align: center

    .. note::

        To learn more about cron expressions and create a schedule in cron tab, `Click here <https://crontab.cronhub.io/>`__.