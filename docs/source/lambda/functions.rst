Overview of Lambda
=================================

Functionality of Lambda Functions
---------------------------------

The fleet of lambda functions will be responsible for end-to-end flow for the Cypienta Correlation Pipeline.

#. **create_mapping:**

    - Gets a file that was uploaded to Field mapping in the UI.
    - Suggests the mapping for the file and responds to the API call.

#. **preprocess_input:**

    - Transforms uploaded file to json format. ``mapping/input/<field-mapping-name>/<filename>`` S3 folder.
    - Map the uploaded file to internal format using corresponding saved field mappings.
    - Save a raw input file to ``mapping/raw/<field-mapping-name>/<filename>`` S3 folder.
    - Save the transformed input file to ``mapping/input/<field-mapping-name>/<filename>`` S3 folder.

#. **skip_input:**

    - Get the transformed input files from the ``input/<field-mapping-name>/`` S3 folder
    - Check if an execution is running for pipeline. If execution is not running, start one. Else, add the current input files to queue.

#. **enrich_with_technique:**

    - Get the input data from the ``input/<field-mapping-name>/`` S3 folder
    - Merge and sort the data from multiple datasources by time and batch the data
    - encode node_features, encode other_attributes_dict, create mappings for internal ids to user given ids, internal ids to data sources.
    - Sanitize it in format as required for cluster model.
    - Enrich input with techniques. If the lookup table does not contain the specific technique, then start technique classification transform job per batch

#. **start_tech_task:**

    - Get the input and output s3 path for the technique classification model.

#. **process_enriched_with_technique:**

    - Get response from technique model and enrich the input with recognized techniques
    - Create input for the clustering model by adding node features if present. And save the resulting file to S3

#. **update_lookup_table:**

    - Update technique lookup table.

#. **create_embedding:**

    - Read the input file saved to S3.
    - Filter alerts that do not have any techniques.
    - Filter batch file that do not have more than a threshold number of alerts.
    - Skip processing current input any further if there are no batches left to process after filtering.
    - Create sequential order of batches to be processed by the pipeline_part_2 DAG.

#. **start_embedding_task:**

    - Get the input and output s3 path for the cluster part 1 model.

#. **process_embedding:**

    - Get response from cluster part 1 model.
    - Create input for the clustering model part 2 by adding node features if present. And save the resulting file to S3

#. **create_cluster:**

    - Read the input file saved to S3 for the current batch. Start clustering part 2 model.

#. **process_cluster:**

    - Read the response from clustering part 2 model.
    - Check if there is another batch that needs to run after the current response.
    - If yes, then create input for the next batch, save to S3.
    - Else, extract agg_alert.json, cluster.json (for internal scratch) to S3, and create input for flow model and save to S3.

.. #. **create_flow:**

..     - Triggered by input saved to s3 for flow model. Create flow transform job

#. **process_flow:**

    - Read response from the flow model. Save the flow_output.json to s3 (for internal scratch)
    - Clean up flow.json, cluster.json for user and save to ``output/`` folder.

#. **create_campaign:**

    - Create or update campaigns on UI.
    - Delete older campaigns if threshold of number of campaigns on UI is reached.
    - Update Pipeline dashboard
    - Update global metrics
    - Update node features and event attributes weights

#. **delete_event:**

    - Delete older campaigns and relevant events from database if threshold of number of campaigns on UI is reached.

#. **save_feedback:**

    - Triggered by cut action performed on UI.
    - Fetch involved events and campaigns from UI and update weights for node and event attributes.
    - Create cluster ticket output for involved clusters, and save feedback.

#. **restore:**

    - Check if there are any snapshots for database to revert to.
    - If yes, then restore the database to the last saved snapshot and restart it.
    - Else, clear database and restart it.

.. 13. **create_jira:**

.. - Read enrich_alert_input.json
.. - Read lookup for the JIRA issue to cluster id.
.. - If the cluster id already has JIRA created, and the status is ``open`` / ``in progress`` / ``to do``, overwrite the description with new details. If the status is not ``open`` / ``in progress`` / ``to do``, then create new JIRA issue with updated summary and description
.. - If the cluster id does not have JIRA created, then create JIRA issue with summary, description and attachment to subset of involved alerts

.. 14. **create_case:**

.. - Read enrich_alert_input.json
.. - Read lookup for the Elastic case to cluster id.
.. - If the cluster id already has case created, and the status is ``open`` / ``in progress``, overwrite the description with new details. If the status is not ``open`` / ``in progress``, then create new case with updated summary and description
.. - If the cluster id does not have case created, then create case with summary, description.
