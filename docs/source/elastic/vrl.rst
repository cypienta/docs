Mapping Elastic data
==============================

To map the data to cypienta input, use the Vector Remap Language (VRL). The output from the VRL mapping must contain ``rule_id``, ``rule_name``, and ``index`` fields.

Using the python script below, we will get 2 files. 

1. **elastic_input.json:** This file can be used as the input to the cypienta product. This file should be uploaded to ``input/`` folder on S3 bucket on which the cypienta product is set up.
2. **alert_to_rule:**: This file must be uploaded to ``elastic/`` folder on S3 bucket on which the cypienta product is set up. If this file is missing, the elastic case created by cypienta lambda function will not attach the alerts to the relevant case.

.. code-block:: 

    import pandas as pd
    import os
    import json

    file_to_read = "vrl_transformed_alerts.json"
    file_to_save = "elastic_input.json"
    rule_mapping_file = "alert_to_rule.json"

    # Read the VRL output file
    df = pd.read_json(file_to_read, lines=True)

    # Keep columns which maps to cypienta input
    keep_cols = ["id", "name", "src", "dst", "time", "tech", "other_attributes_dict"]

    # Columns for the mapping created for alert id to rule id, rule name, and index
    rule_mapping_cols = ["id", "rule_id", "rule_name", "index"]

    empty_rule_id = df["rule_id"].isna().any()
    empty_rule_name = df["rule_name"].isna().any()

    if empty_rule_id:
        print("Found empty rule ids in the alerts.")
    if empty_rule_name:
        print("Found empty rule names in the alerts.")

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

    # save alert id to rule id mapping as json
    df_rule_mapping = df[rule_mapping_cols]
    df_rule_mapping.to_json(rule_mapping_file, orient="records")

    df_input = df[keep_cols]

    df_input.to_json(file_to_save, orient="records")

    os.remove(file_to_read)

    # save cypienta input file as json
    alert_list = json.load(open(file_to_save, "r"))
    json.dump({"input": alert_list}, open(file_to_save, "w"))
