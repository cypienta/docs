Mapping Splunk Data
==============================

To map the data to cypienta input, use the Vector Remap Language (VRL). 

Using the python script below, we will get a file. 

- **splunk_input.json:** This file can be used as the input to the cypienta product. This file should be uploaded to ``input/`` folder on S3 bucket on which the cypienta product is set up.

.. code-block:: 

    import pandas as pd
    import os
    import json

    file_to_read = "vrl_transformed_alerts.json"
    file_to_save = "splunk_input.json"
    rule_mapping_file = "alert_to_rule.json"

    # Read the VRL output file
    df = pd.read_json(file_to_read, lines=True)

    # Keep columns which maps to cypienta input
    keep_cols = ["id", "name", "src", "dst", "time", "tech", "other_attributes_dict"]

    # filter rows which do not have values in required fields
    empty_ids = df["id"].isna().any()
    empty_time = df["time"].isna().any()
    empty_src = df["src"].isna().any()
    empty_dst = df["dst"].isna().any()

    if empty_ids:
        print("Found empty ids in the alerts. Skipping alert.")
    if empty_time:
        print("Found empty time in the alerts. Skipping alert.")
    if empty_src:
        print("Found empty src in the alerts. Skipping alert.")
    if empty_dst:
        print("Found empty dst in the alerts. Skipping alert.")

    df = df[~df["id"].isna()]
    df = df[~df["time"].isna()]
    df = df[~df["dst"].isna()]
    df = df[~df["src"].isna()]

    df_input = df[keep_cols]

    df_input.to_json(file_to_save, orient="records")

    os.remove(file_to_read)

    # save cypienta input file as json
    alert_list = json.load(open(file_to_save, "r"))
    json.dump({"input": alert_list}, open(file_to_save, "w"))
