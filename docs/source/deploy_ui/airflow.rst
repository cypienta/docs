Airflow Configuration
=====================


Airflow Scheduler
-----------------

1.  Go to the Cypienta UI and login with your credentials.

    .. image:: resources/ui_login.png
        :alt: Login to UI
        :align: center


    .. note::
        The default credentials are present in :doc:`start_using` page.

2.  On the Cypienta UI, on the left hand side panel, expand ``Settings`` and click on ``Pipeline Schedule``. The default schedule is setup using a cron tab which will trigger the Pipeline every 5 hours.

    .. image:: resources/bastet_airflow_schedule.png
        :alt: Airflow
        :align: center


3. To edit the pipeline schedule, click on the ``Edit`` button. The schedule can be edited using two different methods. One is a cron tab format and the other is a present schedule.


4. Click on the ``Preset Schedule`` tab to see the preset schedules. Select the drop down ``Select Interval`` to see the different preset schedules.

    .. image:: resources/bastet_airflow_preset_schedule.png
        :alt: Airflow
        :align: center


5. Click on the ``Custom Schedule`` tab to edit the schedule using cron tab expression.

    .. image:: resources/bastet_airflow_custom_schedule.png
        :alt: Airflow
        :align: center

    .. note::

        To learn more about cron expressions and create a schedule in cron tab, `Click here <https://crontab.cronhub.io/>`__.


6. Click on the ``Save`` button to save the schedule.
