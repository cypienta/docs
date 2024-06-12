Subscribe to UI product
==============================

Cypienta User Interface (UI)
----------------------------

To subscribe to Cypienta UI, follow the steps below.

1. Subscribe to the `Cypienta User Interface (UI) <https://www.elastic.co/guide/en/logstash/current/plugins-outputs-s3.html>`__ by clicking on ``Continue to Subscribe`` button.

    .. image:: resources/ui_product.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

    .. image:: resources/confirm_subscribe.png
        :alt: confirm subscribe
        :align: center

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version``. Then click on ``Continue to Launch``

    .. image:: resources/to_launch.png
        :alt: to launch
        :align: center

5. CLick on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

    .. image:: resources/container_images.png
        :alt: container images
        :align: center

    Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

    .. code-block::
        
        aws ecr get-login-password \
            --region us-east-1 | docker login \
            --username AWS \
            --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:nginx-marketv0.0.3,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:marketv0.0.4"    

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the two images are:

    - **Web container image:** 709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:marketv0.0.4
    - **Nginx container image:** 709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:nginx-marketv0.0.3

6. Move to section :doc:`deploy_ui`