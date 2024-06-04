Troubleshoot
========

ERROR: ResourceLimitExceeded for batch transform job instance type.
-------------------------------------------------------------------

Increase the quota for the account using Service Quotas. Navigate to the
AWS console for the region you are using. Search for ``Service Quotas``

1. Search for “Sagemaker” under ``Manage quotas`` and select ``Amazon
   SageMaker`` AWS service. Click on view quotas.

    .. image:: resources/service_quotas.png
        :alt: Service Quota AWS console
        :align: center

2. Search for ``<instance-type> for transform job usage``

    .. image:: resources/quota_instance_types.png
        :alt: Quoatas for Instance types
        :align: center

3. Select the required instance type from the list and click on ``Request
   increase at account level``.
