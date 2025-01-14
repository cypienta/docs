End to End Test (CEF input format)
==================================================

How to test end-to-end
----------------------

1. Navigate to the AWS console and search for ``S3``. Select the S3 bucket
   that you created and navigate to the folder ``mapping/input/cypienta_cef/``.

    .. note::
        The folder structure should be as follows:
        ``s3://{bucket-name}/mapping/input/cypienta_cef/``

2. Sample input json file:

    .. code-block:: JSON

        [
            {
                "_cd": "318:6", 
                "_eventType": "tech", 
                "_time": "1568916650", 
                "id": "318:6", 
                "sourceAddressIPv6": "10.0.0.4", 
                "destinationAddress": "10.0.0.4", 
                "description": "initial execution of malicious document calls wmic to execute the file with regsvr32"
                ...
            }, ...
        ]

    View the `sample input file <https://drive.google.com/file/d/1jVNnNogTKlTW6VIWKByyXmx9fTpyrNlE/view?usp=drive_link>`__ for your reference

    Input data JSON description:

    .. code-block:: JSON
        
        [
            {
                "_cd": "318:6", // id for Event in splunk - optional
                "_eventType": "tech", // event type - optional
                "_time": "1568916650", // time from splunk
                "id": "318:6", // internal id for Event
                "sourceAddressIPv6": "10.0.0.4", // source address
                "destinationAddress": "10.0.0.4", // destination address
                "description": "initial execution of malicious document calls wmic to execute the file with regsvr32" // description of the event
                ... // other cef fields
            }, ...
        ]

    All fields are required unless mentioned otherwise. If the value for the field is not present, keep empty string as value.

3. Upload input json file to the s3 bucket in path: ``s3://{bucket-name}/mapping/input/cypienta_cef/``. The name of the input file does not matter to the end-to-end flow. Note that if you upload a file with the same name, it will be overwritten in S3 bucket.

    1. Once you upload the input file. You can use the Airflow to monitor the flow of your input.

4. Final output will be put on the S3 bucket with prefix ``s3://{bucket-name}/output/``