End to End Test (CEF input format)
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
                    "_cd": "318:6",
                    "_eventType": "err0r",
                    "_source": "ess_content_importer",
                    "_sourceType": "ess_content_importer",
                    "_time": "1568916650",
                    ... // other cef fields
                }
                ...
            ]
        }

    View the `sample input file <https://drive.google.com/file/d/1y35h4FswNJwZvmeoTw6mNBMuocZP9iRI/view?usp=drive_link>`__ for your reference

    Input data JSON description:

    .. code-block:: JSON
        
        {
            "input": [
                {
                    "_cd": "318:6", // internal id for event for splunk
                    "_eventType": "err0r", // event type from splunk
                    "_source": "ess_content_importer", // source from splunk
                    "_sourceType": "ess_content_importer", // sourceType from splunk
                    "_time": "1568916650", // time from splunk
                    ... // other cef fields
                }, ...
            ]
        }

    All fields are required unless mentioned otherwise. If the value for the field is not present, keep empty string as value.

3. Upload input json file to the s3 bucket in path: ``s3://{bucket-name}/input/``. The name of the input file does not matter to the end-to-end flow. Note that if you upload a file with the same name, it will be overwritten in S3 bucket.

    1. Once you upload the input file. You can use the AWS step function to monitor the flow of your input.

    2. You can use the Amazon SageMaker console and navigate to Inference → Batch transform jobs, to view the created jobs for your input.

    3. You can monitor the progress on CloudWatch logs for each lambda function and transform job created.

4. Final output will be put on the S3 bucket with prefix ``s3://alert-detector/output/``