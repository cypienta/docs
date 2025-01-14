Subscribing to Cypienta products on AWS Marketplace
===================================================

Cypienta Correlation Pipeline
-------------------------

1. Subscribe to the `Cypienta Correlation Pipeline <https://aws.amazon.com/marketplace/pp/prodview-cinoiwm3g2sic>`__ by clicking on ``View purchase options`` button.

    .. image:: resources/pipeline_subscribe.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Once you see the ``Continue to Configuration`` button, you are now subscribed to the product and can move to the :doc:`deploy` step.

    .. image:: resources/pipeline_confirm.png
        :alt: confirm subscribe
        :align: center


.. Optional steps
.. ~~~~~~~~~~~~~~

.. If you want to deploy the pipeline manually, follow the steps below to get the list of container images available in the product offering.

.. 1. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

..     .. image:: resources/pipeline_confirm.png
..         :alt: confirm subscribe
..         :align: center

.. 2. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``v0.9``. Then click on ``Continue to Launch``

..     .. image:: resources/pipeline_to_launch.png
..         :alt: to launch
..         :align: center

.. 3. Click on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

..     .. image:: resources/pipeline_container_images.png
..         :alt: container images
..         :align: center

..     Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

..     .. code-block::
        
..         aws ecr get-login-password \
..             --region us-east-1 | docker login \
..             --username AWS \
..             --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
..         CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-cluster-part-1:v0.9.1,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-cluster-part-2:v0.9.1,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-ui-nginx:v0.9.1,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-flow-detector:v0.9.1,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-lambda-function:v0.9.1,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-technique-detector:v0.9.1,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-airflow:v0.9.1,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-ui:v0.9.1"    

..         for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

..     Here the model container images are:

..     -  **Technique container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-technique-detector:v0.9.1.1``

..     -  **Cluster part 1 container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-cluster-part-1:v0.9.1.1``

..     -  **Cluster part 2 container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-cluster-part-2:v0.9.1.1``

..     -  **Flow detector container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-flow-detector:v0.9.1.1``

..     -  **Lambda function container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-lambda-function:v0.9.1.1``

..     -  **Airflow container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-airflow:v0.9.1.1``

..     -  **UI container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-ui:v0.9.1.1``

..     -  **UI-nginx container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/pipeline-ui-nginx:v0.9.1.1``
