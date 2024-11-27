Overview of Airflow DAGs
=================================

Functionality of DAGs
---------------------------------

The fleet of airflow DAGs will be responsible for end-to-end flow for the Cypienta Correlation Pipeline.

#. **s3_trigger:**

    - The DAG is triggered from the periodic schedule in the Cypienta UI.
    - Gets the list of file in the upload folders for multiple data sources ``mapping/input/``. If the list of files is not empty
        then the skip_input DAG is triggered. Else, it exits.

#. **skip_input:**

    - The DAG is triggered by s3_trigger DAG so that the list of input files can be processed.
    - Maintains a queue of input files to be processed by the pipeline.
    - The DAG is triggered from pipeline_part_1, pipeline_part_2, to clear the input queue.

#. **pipeline_part_1:**

    - The DAG is triggered by skip_input DAG to process the input files.
    - It triggers the following tasks in sequence:
        - enrich_with_technique
        - update_lookup_table - trigger update_lookup_table DAG
        - clustering part 1

    - The DAG then puts in the redis queue, the batches that must be processed by the pipeline_part_2 DAG in sequential order, and triggers the pipeline_part_2 DAG.

#. **pipeline_part_2:**

    - The DAG is triggered by pipeline_part_1 DAG to process the batches in sequential order.
    - It reads from the redis queue front and triggers the following tasks in sequence:
        - clustering part 2
        - flow - if the current processed batch is the last batch in the queue
        - create campaign - if the current processed batch is the last batch in the queue
        - retrigger pipeline_part_2 if there are more batches to process
    - Once the current batch is processed successfully, it removes the batch from the redis queue.

#. **snapshot:**

    - The DAG is triggered by the pipeline_part_2 DAG to create a snapshot of the current state of the pipeline.
    - It snapshots the database and restarts it.

#. **restore:**

    - This DAG is triggered by the failure callback of the pipeline_part_1 and pipeline_part_2 DAG.
    - It restores the database to the last saved snapshot and restarts it.

#. **update_lookup_table:**

    - The DAG is triggered by the pipeline_part_1 DAG to update the lookup table for techniques.
