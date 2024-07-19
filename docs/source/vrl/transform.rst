Data transformation / Remodelling
---------------------------

Once you have the Elastic or Splunk logs stored in S3 in JSON Lines format, you
can use the `VRL (Vector Remap
Language) <https://vector.dev/docs/reference/vrl/>`__ tool to transform
your data to meet the format expected by the Cypienta end-to-end
processing.

VRL Transformations
~~~~~~~~~~~~~~~~~~~

To apply a transformation to your log source using the VRL tool, you
need to specify a VRL program file to transform your data as a string in
the ``transforms`` key in your ``log_source.yml`` file. Write your VRL
transformation script and save it as a ``.vrl`` file. Here, ``program.vrl``

Example: parsing JSON
^^^^^^^^^^^^^^^^^^^^^

To look at a simple example. Let's assume the following event.

.. code-block::

    {
    "message": "{\"agent\": {\"build\": {\"original\": \"version: 8.13.4, compiled: Mon May 6 18:00:00 2024, branch: HEAD, commit: 17e171c67d13668a35832f16d541aca13de9df52\"}, \"id\": \"1f0287fe-771f-4c94-88b5-d8d3ac427bd3\", \"type\": \"endpoint\", \"version\": \"8.13.4\"}, \"message\": \"Malicious Behavior Detection Alert: Network Module Loaded from Suspicious Unbacked Memory\", \"@timestamp\": \"2024-05-23T12:54:16.5686093Z\", \"dll\": {\"Ext\": {\"code_signature\": [{\"trusted\": true, \"subject_name\": \"Microsoft Windows\", \"exists\": true, \"status\": \"trusted\"}], \"size\": 1108800, \"relative_file_creation_time\": 1981444.3654891, \"load_index\": 1, \"relative_file_name_modify_time\": 1981444.1782684}, \"path\": \"C:\\\\Windows\\\\System32\\\\winhttp.dll\", \"code_signature\": {\"trusted\": true, \"subject_name\": \"Microsoft Windows\", \"exists\": true, \"status\": \"trusted\"}, \"pe\": {\"file_version\": \"10.0.20348.2400 (WinBuild.160101.0800)\", \"imphash\": \"3760f9eb21fa8e15fefc00a05df20bfd\", \"original_file_name\": \"winhttp.dll\"}, \"name\": \"winhttp.dll\", \"hash\": {\"sha1\": \"5d2a67b664d976a7bb0666371ab9ef83f6f06f2d\", \"sha256\": \"9f37f1c77b3425e024d82f36b84364d1a964ebf0741edd3a8096cd7ae8b17b31\", \"md5\": \"491414a072b93ff2223ef51b9c5e7299\"}}, \"host\": {\"hostname\": \"clauhvmvictim05\", \"os\": {\"Ext\": {\"variant\": \"Windows Server 2022 Standard Evaluation\"}, \"kernel\": \"21H2 (10.0.20348.2402)\", \"name\": \"Windows\", \"family\": \"windows\", \"type\": \"windows\", \"version\": \"21H2 (10.0.20348.2402)\", \"platform\": \"windows\", \"full\": \"Windows Server 2022 Standard Evaluation 21H2 (10.0.20348.2402)\"}, \"ip\": [\"192.168.58.17\", \"fe80::e587:78d4:d27f:eed4\", \"127.0.0.1\", \"::1\"], \"name\": \"clauhvmvictim05\", \"id\": \"141f8f33-9362-44d8-bdca-64376a18240b\", \"mac\": [\"bc-24-11-37-50-9f\"], \"architecture\": \"x86_64\"}, \"threat\": [{\"framework\": \"MITRE ATT&CK\", \"technique\": [{\"reference\": \"https://attack.mitre.org/techniques/T1055/\", \"name\": \"Process Injection\", \"subtechnique\": null, \"id\": \"T1055\"}], \"tactic\": {\"reference\": \"https://attack.mitre.org/tactics/TA0005/\", \"name\": \"Defense Evasion\", \"id\": \"TA0005\"}}], \"event\": {\"severity\": 99, \"code\": \"behavior\", \"risk_score\": 99, \"created\": \"2024-05-23T12:54:16.5686093Z\", \"kind\": \"alert\", \"module\": \"endpoint\", \"type\": [\"info\", \"allowed\"], \"agent_id_status\": \"verified\", \"sequence\": 12543, \"ingested\": \"2024-05-23T12:54:17Z\", \"action\": \"rule_detection\", \"id\": \"NYwRhsgWHlxrlDVV+++++DxY\", \"category\": [\"malware\", \"intrusion_detection\"], \"dataset\": \"endpoint.alerts\", \"outcome\": \"success\"}, \"user\": {\"domain\": \"CLAUHVMVICTIM05\", \"name\": \"Administrator\", \"id\": \"S-1-5-21-1176793669-1443726013-1690302133-500\"}}"
    }


You want to apply following changes to each event:

-  Parse the raw ``message`` string to JSON, and explode the fields to the
   top level
-  Get unique ``id`` from the event
-  Get ``time`` from the event
-  Get ``src`` and ``dst`` from the host details
-  Get ``name`` from the message present in the event
-  If the MITRE ATTACK threats are detected, append techniques to ``tech``
-  For ``other_attributes_dict``, flatten all the important keys present
   in the parsed json ``log``

program.vrl

.. code-block::

    log = parse_json!(.message)
    .id = log.event.id
    .time = to_unix_timestamp(parse_timestamp!(log."@timestamp", format: "%Y-%m-%dT%H:%M:%S%.fZ"))
    .src = log.host.hostname
    .dst = log.host.hostname
    .name = log.message
    tech = []
    if exists(log.threat) {
    for_each(array!(log.threat)) -> |_index, threat| {
        for_each(array!(threat.technique)) -> |_index, technique| {
        tech = push(tech, technique.id)
        }
    }
    }
    .tech = tech

    other_attributes_dict = {}
    if exists(log.Events) {
    other_attributes_dict.Events = flatten!(log.Events)
    }

    if exists(log.process) {
    other_attributes_dict.process = flatten!(log.process)
    }

    if exists(log.dll) {
    other_attributes_dict.dll = flatten!(log.dll)
    }

    if exists(log.rule) {
    other_attributes_dict.rule = flatten!(log.rule)
    }

    if exists(log.endpoint) {
    other_attributes_dict.endpoint = flatten!(log.endpoint)
    }

    if exists(log.event) {
    other_attributes_dict.event = flatten!(log.event)
    }

    if exists(log.file) {
    other_attributes_dict.file = flatten!(log.file)
    }

    if exists(log.Memory_protection) {
    other_attributes_dict.Memory_protection = flatten!(log.Memory_protection)
    }

    if exists(log.Target) {
    other_attributes_dict.Target = flatten!(log.Target)
    # .other_attributes_dict = merge(., file)
    
    }

    .other_attributes_dict = flatten(other_attributes_dict)

    del(.file)
    del(.message)
    del(.host)
    del(.timestamp)
    del(.source_type)

.. note::
    This VRL transform script is specific to this particular structure
    of the event and used as example. The mappings from events to input
    structure of the Cypienta product could vary for different structures.

log_source.yml

.. code-block::

    # Define the source to read from a local file
    sources:
    local_file:
        type: file
        include: ["./elastic_input.json"]
        read_from: beginning
        data_dir: "./"
        max_line_bytes: 1024000 # Increase the maximum allowed line length to 1MB

    # Define the transform to remap the log data
    transforms:
    remap:
        type: remap
        inputs: ["local_file"]
        file: "program.vrl"

    # Define the sink to write the transformed data to a new file
    sinks:
    file_sink:
        type: file
        inputs: ["remap"]
        path: "./vrl_transformed_log.json"
        encoding:
        codec: json

.. note::
    This log_source.yml is configured to read a local file, transform
    it using ``program.vrl`` and output the results to another local file.
    Configure sources and sinks in the yml as required.

The resulting event:

.. code-block::
    
    {
        "dst": "clauhvmvictim05",
        "id": "NYwRhsgWHlxrlDVV+++++DxY",
        "name": "Malicious Behavior Detection Alert: Network Module Loaded from Suspicious Unbacked Memory",
        "other_attributes_dict": {
            "dll.Ext.code_signature": [
                {
                    "exists": true,
                    "status": "trusted",
                    "subject_name": "Microsoft Windows",
                    "trusted": true
                }
            ],
            "dll.Ext.load_index": 1,
            "dll.Ext.relative_file_creation_time": 1981444.3654891,
            "dll.Ext.relative_file_name_modify_time": 1981444.1782684,
            "dll.Ext.size": 1108800,
            "dll.code_signature.exists": true,
            "dll.code_signature.status": "trusted",
            "dll.code_signature.subject_name": "Microsoft Windows",
            "dll.code_signature.trusted": true,
            "dll.hash.md5": "491414a072b93ff2223ef51b9c5e7299",
            "dll.hash.sha1": "5d2a67b664d976a7bb0666371ab9ef83f6f06f2d",
            "dll.hash.sha256": "9f37f1c77b3425e024d82f36b84364d1a964ebf0741edd3a8096cd7ae8b17b31",
            "dll.name": "winhttp.dll",
            "dll.path": "C:\\Windows\\System32\\winhttp.dll",
            "dll.pe.file_version": "10.0.20348.2400 (WinBuild.160101.0800)",
            "dll.pe.imphash": "3760f9eb21fa8e15fefc00a05df20bfd",
            "dll.pe.original_file_name": "winhttp.dll",
            "event.action": "rule_detection",
            "event.agent_id_status": "verified",
            "event.category": [
                "malware",
                "intrusion_detection"
            ],
            "event.code": "behavior",
            "event.created": "2024-05-23T12:54:16.5686093Z",
            "event.dataset": "endpoint.alerts",
            "event.id": "NYwRhsgWHlxrlDVV+++++DxY",
            "event.ingested": "2024-05-23T12:54:17Z",
            "event.kind": "alert",
            "event.module": "endpoint",
            "event.outcome": "success",
            "event.risk_score": 99,
            "event.sequence": 12543,
            "event.severity": 99,
            "event.type": [
                "info",
                "allowed"
            ]
        },
        "src": "clauhvmvictim05",
        "tech": [
            "T1055"
        ],
        "time": 1716468856
    }

Writing transformation VRL expressions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The input to your VRL expression is a single record from your data
source. The output of the VRL expression is the transformed record.




Using an AI model to generate mappings
~~~~~~~~~~~~~~~~~~~

https://github.com/cypienta/data_mapper_model

