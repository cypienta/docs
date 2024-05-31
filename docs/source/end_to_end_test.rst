Sample Test
==================================================

How to test end-to-end
--------------------------

1. Navigate to the AWS console and search for ``S3``. Select the S3 bucket
   that you created, and click on ``Create folder``. Give a name of folder
   as ``input`` and create the folder.
2. Sample input json file:

    View the `sample input
    file <https://drive.google.com/file/d/1b9KLQ5k-259zklX1u56Gpk255SUUFeXP/view?usp=drive_link>`__
    for your reference

    Input data JSON description:

    All fields are required unless mentioned otherwise.

3. Upload input json file to the s3 bucket in path (eg:
   s3://{bucket-name}/input/). The name of the input file does not
   matter to the end-to-end flow. Note that if you upload a file with
   the same name, it will be overwritten in S3 bucket.

    1. Once you upload the input file. Lets say ``input.json``. Then the
        control flow will be as follows:

        -  Enrich_with_technique: lambda function
        -  Transform-job-tech-{unique-id}: Batch transform job

            -  Reads input from:
                ``{bucket}/intermediate/{unique-id}/input_classification.json``
            -  Output to:
                ``{bucket}/response/classification_out/{unique-id}/input_classification.json.out``

        -  Process_enriched_with_technique: lambda function
        -  Create_cluster: lambda function
        -  Transform-job-cluster-{unique-id}: Batch transform job

            -  Reads input from:
                ``{bucket}/output/classification/{unique-id}/input.zip``
            -  Output to:
                ``{bucket}/response/cluster_out/{unique-id}/input.zip.out``

        -  Process_cluster: lambda function
        -  Create_flow: lambda function
        -  Transform-job-flow-{unique-id}: Batch transform job

            -  Reads input from:
                ``{bucket}/output/cluster/{unique-id}/input_flow.json``
            -  Output to:
                ``{bucket}/response/flow_out/{unique-id}/input_flow.json.out``

        -  Process_flow: lambda function

    2. You can use the Amazon SageMaker console and navigate to Inference
        → Batch transform jobs, to view the created jobs for your input.

    3. You can monitor the progress on CloudWatch logs for each lambda
        function and transform job created.

4. Wait for a complete output to show up on the S3 bucket.
   (s3://alert-detector/output/flow/{unique-id}/)