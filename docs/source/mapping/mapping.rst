Map Alert fields to Cypienta Internal Format
============================================

1.  Go to the Cypienta UI and login with your credentials.

    .. image:: resources/ui_login.png
        :alt: Login to UI
        :align: center


    .. note::
        The default ``Username`` is ``maestro`` and the default ``Password`` is ``changemenow``

2.  On the left hand side panel, click on ``Add Alerts``.

    .. image:: resources/add_alerts.png
        :alt: Add Alerts
        :align: center

3.  Drag and drop a file or click on the drag and drop area to upload a file. Once the file is selected, click on ``Upload File``.

    .. note::
        The maximum file size that can be uploaded is 10MB. The file should be in CSV/XML/JSON format.

        CSV: The CSV format file must have filename with extension as ``.csv``. The first row will be considered as the header row and there must be atleast 1 alert in the file.
        
        JSON: The JSON format file must have filename with extension as ``.json``. The file must have a json list of alerts format and must contain atleast 1 alert.

        XML: The XML format file must have filename with extension as ``.xml``. The file must have a list element for ``alerts`` and must contain atleast 1 alert.

4. Once the file is uploaded, the system will automatically map and suggest the fields to the internal format.

    .. image:: resources/alerts_mapping.png
        :alt: Alerts Mapping
        :align: center

5. There are 5 required fields that must be mapped to the internal format. The fields are:

    -  ``Id``: This field is used to uniquely identify the alert.
    -  ``Time``: This field is used to represent the time of the alert and must be in datetime format.
    -  ``Name``: Human readable alert text.
    -  ``Src``: Source of the alerts for network alerts.
    -  ``Dst``: Destination of the alerts for network alerts.

    The remaining fields are optional and can be mapped as per the requirement.

    -  ``Event_feature``
    -  ``Node_feature``

    The mapping of fields to internal is put in a interactive UI element partitioned in 2 different sections. The left section shows the fields from the uploaded file which are currently not selected for any internal mapping and the right section shows the fields that are chosen for the internal format.
    The user may click on the plus button to map a field to the internal format. The user may also click on the minus button to remove a field from the internal format.
    To select all the fields currently in the ``Unused fields`` section, click on ``Choose all`` button. To remove all the fields currently in the ``Chosen fields`` section, click on ``Remove all`` button.
    
    The top 3 values for the fields in the ``Chosen fields`` section are displayed as sample values, if available.

    All chosen fields for required field mappings have a drop down for selecting the priority. If user wants to select multiple options for fetching the field value for the required fields, the user may select the priority for the field. The priority is used to select the value of the field from the multiple options available in the alert. Field value with priority 1 is given the highest priority and if fount to be not empty, other values are ignored.

    For node feature field mapping, each selected field will have a drop down with options ``src``, ``dst``, and ``both``. The user must select any one option for each field to associate the node feature field to the source, destination or both.


    .. image:: resources/node_feature.png
        :alt: Alerts Mapping
        :align: center

6. Once all the required fields have atleast 1 chosen field. Click on ``Activate mapping for ingestion`` to save the mapping and start the ingestion process.

    .. image:: resources/activate_mapping.png
        :alt: Activate Mapping
        :align: center

7. Click on ``OK`` to confirm saving the mapping.
    
    .. image:: resources/save_mapping.png
        :alt: Confirm Mapping
        :align: center

8. Give a unique name for the mapping and then click ``OK``. A recommended mapping name is a source type of the alerts.

    .. image:: resources/save_mapping_name.png
        :alt: Mapping Name
        :align: center

9. Once the mapping is saved, an alert with a bucket prefix will appear. Note down this bucket prefix for future use. The alerts can then directly be uploaded to the bucket prefix path to automatically map the alerts to the internal format and start the ingestion process.
   Click on ``OK`` to close the alert.

    .. image:: resources/mapping_saved.png
        :alt: Bucket Prefix
        :align: center

10. Another alert shows that the pipeline have been triggered with the uploaded input file. Click on ``OK`` to close the alert.

    .. image:: resources/pipeline_triggered.png
        :alt: Pipeline Triggered
        :align: center