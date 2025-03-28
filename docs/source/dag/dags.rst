Overview of Airflow DAGs
=================================

Functionality of DAGs
---------------------------------

The fleet of airflow DAGs will be responsible for end-to-end flow for the Cypienta Correlation Pipeline.

#. **file_polling:**

    - The DAG is triggered from the periodic schedule in the Cypienta UI.
    - Gets the list of file in the upload folders for multiple data sources ``input/<data_source_name>/``. If the list of files is not empty then the schema_matcher_and_classifier DAG is triggered. Else, it exits.
    - If there is an error while processing the files in the DAG, the ``file_polling_recovery`` DAG is triggered. And, the ``file_polling`` DAG will be blocked until the ``file_polling_recovery`` DAG is completed successfully.
    - This DAG will record the order of batch of files.
    - The DAG run concurrency is limited to 1 DAG run at a time.

#. **file_polling_recovery:**

    - The DAG is triggered by the failure callback of the `file_polling` DAG.
    - It will be triggered only if the ``file_polling`` DAG is failed.
    - It will recover from failure by putting the processing files back to the original location.
    - If this DAG fails, it will be retried until success.
    - The DAG run concurrency is limited to 1 DAG run at a time.

    .. note::
        The ``file_polling_recovery`` DAG will be triggered infinitely until the ``file_polling`` DAG is completed successfully.

#. **schema_matcher_and_classifier:**

    - The DAG is triggered by file_polling DAG.
    - It starts a concurrent task for each file in the batch. The concurrency is currently limited to 4 tasks, 1 DAG run at a time.
    - The ``schema_matcher_and_classifier`` task will transform the raw data into internal format for cypienta pipeline, and enrich the input data with MITRE ATT&CK techniques.
    - If there are no alerts to be processed, the DAG will exit and skip the current batch by triggering the ``skip_batch`` DAG.
    - If there is an error in the task for schema matcher, the task will be retried 5 times with exponential backoff.
    - If the error persists, the task will be marked as failed, trigger the ``skip_batch`` DAG and the DAG will exit.
    - The DAG run once completed, it will trigger the ``aggregator`` DAG.

#. **aggregator:**

    - The DAG is triggered by ``schema_matcher_and_classifier`` DAG to process the input files.
    - It takes as an input a volume file path prefix to get list of files to be aggregated.
    - It will aggreagte the data from the list of files, aggregate the data, chunk the data, and save the final output data to the volume.
    - The concurrency is limited to 1 DAG run at a time with concurrency limit of 4.
    - If there is an error in the task for aggregator, the task will be retried 5 times with exponential backoff.
    - If the error persists, the task will be marked as failed, trigger the ``skip_batch`` DAG and the DAG will exit.
    - It will get list of clustering agents from the volume and it will trigger the ``clustering`` DAG per chunk for each clustering agent.

#. **clustering:**

    - The DAG is triggered by ``aggregator`` DAG to process the a chunk in a clustering agent.
    - It will start the clustering task for each chunk of aggregated data.
    - After completing the clustering task, it will start concurrent task to check which sequencer data is available and can be used to start the sequencer model, i.e. trigger the ``sequencer`` DAG. And, it will process the cluster output and create campaigns on the Cypienta UI.
    - If the sequencer data is available, it will start the sequencer model one chunk per DAG run.
    - If the sequencer data is not available, it will skip the task in the DAG.
    - The concurrency is limited to 10 DAG runs at a time with concurrency limit of tasks as 4.
    - If any task in clustering DAG fails, it will be retried 5 times with exponential backoff. after 5 retries, it will trigger the ``skip_batch`` DAG for the current chunk and clustering agent and skip it in any further processing.

#. **sequencer:**

    - The DAG is triggered by ``clustering`` DAG to process a chunk in a clustering agent.
    - Internally, this will be triggered based on the batch order and the chunk order determined by the ``aggregator`` DAG.
    - The clustering DAG will only trigger the sequencer DAG if the sequencer data is available.
    - The sequencer data is fundamentally a chunk of data from cluster output in a fixed window size of 5000 clusters.
    - It is possible that one chunk of cluster output has multiple chunks of sequencer data, which will result into multiple DAG runs for ``sequencer`` DAG.
    - Once the sequencer model is completed, it will trigger the task to create flow campaigns on the Cypienta UI.

#. **skip_batch:**

    - The DAG is triggered by ``schema_matcher_and_classifier`` DAG if there are no alerts to be processed or if there is an error in the ``schema_matcher_and_classifier`` DAG.
    - The DAG is triggered by ``aggregator`` DAG if there is an error in the ``aggregator`` DAG.
    - The DAG is triggered by ``clustering`` DAG if there is an error in the ``clustering`` DAG.
    - The DAG will check the source of the trigger and decide if the entire batch or a specific chunk is to be skipped for any further processing. And makes sure that the subsequent pipelne runs are not blocked by any failed task.
    - If there are any pending chunks that can be processed now through sequencer model given that the current batch or chunk is marked as failed, then it will trigger the ``start_sequencer`` DAG.
