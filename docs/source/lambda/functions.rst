Overview of Lambda
=================================

Functionality of Lambda Functions
---------------------------------

The fleet of lambda functions will be responsible for end-to-end flow for the Cypienta Sagemaker products.

1. **splunk_input:**

- Get the input from splunk pushed to S3 path ``splunk_input/input`` and chunk it to tranform using VRL.
- Merge back transformed input after vrl_lambda processes all chunks and put it in the ``input/`` folder.

2. **vrl_lambda:**

- Transform data from CIM to CEF mapping.
- Save transformed alerts to S3.

3. **enrich_with_technique:**

- Get the input data from the ``input/`` S3 folder
- Chunk the input, sanitize it in format as required for cluster model, encode node_features, encode other_attributes_dict, create mappings for internal ids to user given ids, mappings for chunk unique id to internal ids.
- Enrich input with techniques. If the lookup table does not contain the specific technique, then start technique classification transform job per chunk

4. **process_enriched_with_technique:**

- Get response from technique transform job and enrich the input with recognized techniques
- Create input for the clustering model by adding node features if present. And save the resulting file to S3

5. **update_lookup_table:**

- Update technique lookup table.

6. **create_cluster:**

- Read the input file saved to S3. If this is the first batch for the input file, then start clustering transform job. Else skip the file.

7. **process_cluster:**

- Read the response from clustering model.
- Check if there is another batch that needs to run after the current response. If yes, then create input for the next batch, save to S3, and start clustering transform job. Else, extract agg_alert.json, cluster.json (for internal scratch) to S3, and create input for flow model and save to S3.

8. **create_flow:**

- Triggered by input saved to s3 for flow model. Create flow transform job

9. **process_flow:**

- Read response from the flow model. Save the flow_output.json to s3 (for internal scratch)
- Clean up flow.json, cluster.json for user and save to ``output/`` folder.
- Create enrich_alert_input.json and save to S3 (for internal scratch)

10. **create_campaign:**

- Read enrich_alert_input.json and create campaigns on UI

11. **create_jira:**

- Read enrich_alert_input.json
- Read lookup for the JIRA issue to cluster id.
- If the cluster id already has JIRA created, and the status is ``open`` / ``in progress`` / ``to do``, overwrite the description with new details. If the status is not ``open`` / ``in progress`` / ``to do``, then create new JIRA issue with updated summary and description
- If the cluster id does not have JIRA created, then create JIRA issue with summary, description and attachment to subset of involved alerts

12. **create_case:**

- Read enrich_alert_input.json
- Read lookup for the Elastic case to cluster id.
- If the cluster id already has case created, and the status is ``open`` / ``in progress``, overwrite the description with new details. If the status is not ``open`` / ``in progress``, then create new case with updated summary and description
- If the cluster id does not have case created, then create case with summary, description.

13. **save_feedback:**

- Triggered by cut action performed on UI.
- Fetch involved events and campaigns from UI and update weights for node and event attributes.
- Create cluster ticket output for involved clusters, and save feedback.
