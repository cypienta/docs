End to End Test
==================================================

How to test end-to-end
--------------------------

1. Navigate to the AWS console and search for ``S3``. Select the S3 bucket
   that you created, and click on ``Create folder``. Set the name of the folder as ``input`` and create the folder.
2. Sample input json file:

    .. code-block:: JSON

        {
            "input": [
                {
                    "id": 25485,
                    "src": "Internal_User_1",
                    "dst": "ServerA",
                    "time": 1.2741200091934108,
                    "tech": [
                        "T1490"
                    ],
                    "name": " ET DNS Query for .cc TLD ",
                    "other_attributes_dict": {
                        "priority": 1
                    }
                }
                ...
            ]
        }

    View the `sample input file <https://drive.google.com/file/d/1dZl_M3eBth-o6q_X0jY3K7RU_s3nD9IY/view?usp=drive_link>`__ for your reference

    Input data JSON description:

    .. code-block:: JSON
        
        {
            "input": [
                {
                    "id": int, // alert id
                    "time": float, // timestamp of the alert
                    "src": string, // IP address/hostname of the source
                    "dst": string, // IP address/hostname of the destination
                    "name": string, // text value of the alert
                    "tech": [
                        string //optional. keep empty list if there is no technique label.
                    ] // List of technique labels for the alert.
                    "other_attributes_dict": {
                        "priority": float, // optional field of type FLOAT. Priority of the alert Optional field. Only include if the value is not null. (doesnt matter if ascending or descending in importance, as long as its consistent) 
                        "port": int, // optional. Must be an integer 0-65535. Port of the destination. Optional field. Only include if the value is not null.
                        "url": string, // optional. URL of the alert (if applicable). Optional field of type STRING. Only include if the value is not null.
                        "user_agent": string, // optional. User agent of the alert (if applicable). Optional field of type STRING. Only include if the value is not null.
                        "cert": string // optional. Certificate of the alert (if applicable). Optional field of type STRING. Only include if the value is not null.
                    }
                }, ...
            ],
            "node_feature": { // Optional, Add an optional number of nodes, with an optional number of keys per node, make sure to use the same node key/id in its relevant events, feel free to add any features to every node, exclude the features that are non existent for that node
                "ServerA": {
                    "userid": "machine_1",
                    "user_group": "HR",
                    "OS": "Linux",
                    "Internal/External": "Internal"
                },
                "Internal_User_1": {
                    "userid": "machine_2",
                    "user_group": "EMP",
                    "OS": "Windows11"
                },...
            }, //optional. list of node features
        }

    All fields are required unless mentioned otherwise.

3. Upload input json file to the s3 bucket in path: ``s3://{bucket-name}/input/``. The name of the input file does not matter to the end-to-end flow. Note that if you upload a file with the same name, it will be overwritten in S3 bucket.

    1. Once you upload the input file. Lets say ``input.json``. Then the control flow will be as follows:

        -  Enrich_with_technique: lambda function
        -  Transform-job-tech-{unique-id}: Batch transform job

            -  Reads input from: ``{bucket}/intermediate/{unique-id}/input_classification.json``
            -  Output to: ``{bucket}/response/classification_out/{unique-id}/input_classification.json.out``

        -  Process_enriched_with_technique: lambda function
        -  Create_cluster: lambda function
        -  Transform-job-cluster-{unique-id}: Batch transform job

            -  Reads input from: ``{bucket}/output/classification/{unique-id}/input.zip``
            -  Output to: ``{bucket}/response/cluster_out/{unique-id}/input.zip.out``

        -  Process_cluster: lambda function
        -  Create_flow: lambda function
        -  Transform-job-flow-{unique-id}: Batch transform job

            -  Reads input from: ``{bucket}/output/cluster/{unique-id}/input_flow.json``
            -  Output to: ``{bucket}/response/flow_out/{unique-id}/input_flow.json.out``

        -  Process_flow: lambda function

    2. You can use the Amazon SageMaker console and navigate to Inference → Batch transform jobs, to view the created jobs for your input.

    3. You can monitor the progress on CloudWatch logs for each lambda function and transform job created.

4. Wait for a complete output to show up on the S3 bucket. ``s3://alert-detector/output/flow/{unique-id}/``