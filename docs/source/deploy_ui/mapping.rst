Map Alert fields to Cypienta Internal Format
============================================

1.  Go to the Cypienta UI and login with your credentials.

    .. image:: resources/ui_login.png
        :alt: Login to UI
        :align: center


    .. note::
        The default ``Username`` is ``cypienta`` and the default ``Password`` is ``cypienta``

2.  On the left hand side panel, expand the ``Data Management`` and click on ``Sources and Destinations``.

    .. image:: resources/sources_page.png
        :alt: Sources page
        :align: center

3.  Click on ``Add Custom Data Source`` button on the top right corner of the section.

    .. image:: resources/add_alerts.png
        :alt: Sources page
        :align: center

4.  Drag and drop a file or click on the drag and drop area to upload a file. Once the file is selected, click on ``Upload File``.

    .. note::
        The maximum file size that can be uploaded is 10MB. The file should be in CSV/XML/JSON format.

        CSV: The CSV format file must have filename with extension as ``.csv``. The first row will be considered as the header row and there must be atleast 1 alert in the file.
        
        JSON: The JSON format file must have filename with extension as ``.json``. The file must have a json list of alerts format and must contain atleast 1 alert.

        JSONLines: The JSONLines format file must have filename with extension as ``.jsonl``. The file must have a json list of alerts format and must contain atleast 1 alert. In case you have jsonlines format file with extension as ``.json``, the file will be detected as JSON lines format file automatically.

        XML: The XML format file must have filename with extension as ``.xml``. The file must have a list element for ``alerts`` and must contain atleast 1 alert.

5. Once the file is uploaded, the system will automatically map and suggest the fields to the internal format.

    .. image:: resources/alerts_mapping.png
        :alt: Alerts Mapping
        :align: center

    .. note::
        The system will automatically map the fields to the internal format using the field names of only the first alert. The sample values shown in the right section are from the first 3 alerts if they are available. So, make sure the first alert has all the fields that are required to map to the internal format, it could be set as empty values if the field is not always present, and set priority for the fields.

6. There are 5 required fields that must be mapped to the internal format. The fields are:

    -  ``Time``: This field is used to represent the time of the alert and must be in datetime format.
    -  ``Name``: Human readable alert text.
    -  ``Entities``: Entities of the alerts for network alerts. These are source and destination entities of the alerts.

    The remaining fields are optional and can be mapped as per the requirement.

    -  ``id``
    -  ``Tech``
    -  ``Secondary Entities``
    -  ``Event Feature``
    -  ``Entity Feature``

    The mapping of fields to internal is put in a interactive UI element partitioned in 2 different sections. The left section shows the fields from the uploaded file which are currently not selected for any internal mapping and the right section shows the fields that are chosen for the internal format.
    The user may click on the plus button to map a field to the internal format. The user may also click on the minus button to remove a field from the internal format.
    To select all the fields currently in the ``Unused fields`` section, click on ``Choose all`` button. To remove all the fields currently in the ``Chosen fields`` section, click on ``Remove all`` button.
    
    The top 3 values for the fields in the ``Chosen fields`` section are displayed as sample values, if available.

    Few section allow chosen fields to have a drop down for selecting the priority. If user wants to select multiple options for fetching the field value for the required fields, the user may select the priority for the field. The priority is used to select the value of the field from the multiple options available in the alert. Field value with priority 1 is given the highest priority and if fount to be not empty, other values are ignored.

    For entity feature field mapping, each selected field will have a drop down with options ``All`` or specific ``Entities`` chosen fields. The user must select any one option for each field to associate the node feature field to the corresponding entities.


    .. image:: resources/node_feature.png
        :alt: Alerts Mapping
        :align: center

    For event feature field mapping, each selected field will have a drop down with options ``Keep as Source`` or specific ECS feature field key names. The user must select any one option for each field to associate the event feature field to the corresponding ECS feature field key name. By selecting ``Keep as Source``, the field value will be kept as is in the internal format as shown on the UI.


    .. image:: resources/event_feature.png
        :alt: Event Feature
        :align: center

7. Once all the required fields have atleast 1 chosen field. Click on ``Activate mapping for ingestion`` to save the mapping and start the ingestion process.


8. Click on ``OK`` to confirm saving the mapping.
    
    .. image:: resources/save_mapping.png
        :alt: Confirm Mapping
        :align: center

9. Give a unique name for the mapping and then click ``OK``. A recommended mapping name is a source type of the alerts.

    .. image:: resources/save_mapping_name.png
        :alt: Mapping Name
        :align: center

10. On the next page, you can see the aggregation settings for the mapping. You can select the aggregation fields from the mapped fields in event features. And select a custom time window for the aggregation of alerts. Then click on the ``Save Mapping with Aggregation Settings`` button.

    .. image:: resources/aggregation_settings.png
        :alt: Aggregation Settings
        :align: center

11. An alert box with a bucket prefix will appear. Note down this bucket prefix for future use. The alerts can then directly be uploaded to the bucket prefix path to automatically map the alerts to the internal format and start the ingestion process.
   Click on ``OK`` to close the alert.

    .. image:: resources/mapping_saved.png
        :alt: Bucket Prefix
        :align: center

12. The next alerts box gives an option to navigate to the pipeline management page. Click on ``OK`` to navigate to it. You can click on ``Trigger Pipeline Now`` button to trigger the pipeline.

    .. image:: resources/pipeline_management_alert.png
        :alt: Pipeline Triggered
        :align: center

    .. image:: resources/pipeline_management.png
        :alt: Pipeline Triggered
        :align: center
