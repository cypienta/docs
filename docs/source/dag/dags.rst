Overview of Airflow DAGs
=================================

Functionality of DAGs
---------------------------------

The fleet of airflow DAGs will be responsible for end-to-end flow for the Cypienta Correlation Pipeline.

#. **s3_trigger:**

    - The DAG is triggered from the periodic schedule in the Cypienta UI.
    - Gets the list of file in the upload folders for multiple data sources ``mapping/input/``. If the list of files is not empty then the skip_input DAG is triggered. Else, it exits.

#. **skip_input:**

    - The DAG is triggered by s3_trigger DAG so that the list of input files can be processed.
    - Maintains a queue of input files to be processed by the pipeline.
    - The DAG is triggered from pipeline_part_1, pipeline_part_2, to clear the input queue.

#. **pipeline_part_1:**

    - The DAG is triggered by skip_input DAG to process the input files.
    - It triggers the following tasks in sequence:

        - enrich_with_technique
        - update_lookup_table - trigger update_lookup_table DAG

    - The DAG then triggers pipeline_part_2 concurrently, one for each clustering agent to be processed.

#. **pipeline_part_2:**

    - The DAG is triggered by pipeline_part_1 DAG to process the batches per clustering agent.
    - It triggers the following tasks in sequence:

        - clustering part 1 - batches for clustering part 1 are ran concurrently
    
    - The DAG then triggers pipeline_part_3 concurrently for per clustering agent to be processed. Each clustering agent DAG run will run only single batch of data in sequential manner.

#. **pipeline_part_3:**

    - The DAG is triggered by pipeline_part_2 DAG to process the batches in sequential order per clustering agent.
    - It triggers the following tasks in sequence:

        - clustering part 2
        - retrigger pipeline_part_3 if there are more batches to process

    - Once the current batch is processed successfully, it triggers the pipeline_part_4 DAG for the pertinent clustering agent.

#. **pipeline_part_4:**

    - The DAG is triggered by pipeline_part_3 DAG.
    - It triggers the following tasks in sequence:

        - flow - batches for flows are ran concurrently
        - create campaign

#. **snapshot:**

    - The DAG is triggered by the pipeline_part_4 DAG to create a snapshot of the current state of the pipeline.
    - It snapshots the database and restarts it.

#. **restore:**

    - This DAG is triggered by the failure callback of the pipeline_part_1, pipeline_part_2, pipeline_part_3, and pipeline_part_4 DAG.
    - It restores the database to the last saved snapshot and restarts it.

#. **update_lookup_table:**

    - The DAG is triggered by the pipeline_part_1 DAG to update the lookup table for techniques.
