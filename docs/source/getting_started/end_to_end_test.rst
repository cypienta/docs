End to End Test (non CEF input format)
==================================================

How to test end-to-end
----------------------

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

3. Update the environment variables for the lambda functions: ``enrich_with_technique`` and ``process_flow``, and update variable ``map_cef_to_internal`` to value set as ``false``.

    .. note::
        To access the environment variable for lambda functions, navigate to the AWS console and search for ``Lambda``. Select the lambda function that you want to edit. Click on the ``Configuration`` tab and select ``Environment variables`` 
        from the left panel under ``Configuration``. Click on ``Edit`` button to open the edit page and update the pertinent values.

    .. note::
        If the node_feature are already in encoded format, skip the encoding of node features by updating environment variable to ``enrich_with_technique``. Update the value for ``encode_node_feature`` to ``false``.

3. Upload input json file to the s3 bucket in path: ``s3://{bucket-name}/input/``. The name of the input file does not matter to the end-to-end flow. Note that if you upload a file with the same name, it will be overwritten in S3 bucket.

    1. Once you upload the input file. You can use the AWS step function to monitor the flow of your input.

    2. You can use the Amazon SageMaker console and navigate to Inference → Batch transform jobs, to view the created jobs for your input.

    3. You can monitor the progress on CloudWatch logs for each lambda function and transform job created.

4. Final output will be put on the S3 bucket with prefix ``s3://alert-detector/output/``