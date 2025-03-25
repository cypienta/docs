Overview of Lambda
=================================

Functionality of Lambda Functions
---------------------------------

The fleet of lambda functions will be responsible for end-to-end flow for the Cypienta Correlation Pipeline.

#. **schema_matcher_and_classifier:**

    - This lambda function will be triggered by in the ``schema_matcher_and_classifier`` DAG.
    - It will map the raw data to the internal format for cypienta pipeline, and enrich the input data with MITRE ATT&CK techniques.
    - It will reduce the number of alerts to be enriched by using a cache.
    - It will generate a unique id for each alert and store the mapping for the same.

#. **aggregator:**

    - This lambda function will be triggered by in the ``aggregator`` DAG.
    - It will get the data from the list of files, aggregate the data, chunk the data, and save the final output data to the volume.
    - It will prepare the queue for batch order and chunk order for the sequencer model input.

#. **process_cluster_output:**

    - This lambda function will be triggered by in the ``clustering`` DAG.
    - It will process the cluster output and create campaigns on the Cypienta UI.

#. **start_sequencer:**

    - This lambda function will be triggered by in the ``clustering`` DAG.
    - It will check if the sequencer data is available in the volume to start the sequencer model given that the current chunk has completed clustering successfully.
    - If the sequencer data is available, it will prepare the input data for the sequencer model.
    - It will check if there are any next pending sequencer input data that can be used to start the sequencer model.
    - If there are, then it will prepare the input data for the sequencer model.
    - The list of all the sequencer input data is given as output to be processed in individual DAG runs for the ``sequencer`` DAG.

#. **process_sequencer_output:**

    - This lambda function will be triggered by in the ``sequencer`` DAG.
    - It will process the sequencer output and create flow campaigns on the Cypienta UI.

#. **skip_batch:**

    - This lambda function will be triggered by in the ``skip_batch`` DAG.
    - In case of error from ``schema_matcher_and_classifier`` or ``aggregator`` DAG, it will skip the entire batch and mark the current batch as failed.
    - In case of error from ``clustering`` DAG, it will skip the specific chunk for a clustering agent and mark it as failed.
    - After marking the current batch or chunk as failed, it will check if there are any pending chunks that can be processed now through sequencer model given that the current batch or chunk is marked as failed.
    - If there are, then it will prepare the input data for the sequencer model and trigger the ``start_sequencer`` lambda function.
