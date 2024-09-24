Subscribing to Cypienta products on AWS Marketplace
===================================================

Subscribe to Machine Learning models
-------------------------

1. Subscribe to the `Cypienta MITRE ATTACK Flow Detector (TEMP) <https://aws.amazon.com/marketplace/pp/prodview-tmtfhgbkm63tm>`__ by clicking on ``Continue to Subscribe`` button.

    .. image:: resourcestrial_product.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

    .. image:: resources/trial_confirm_subscribe.png
        :alt: confirm subscribe
        :align: center

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``trialv0.0.4``. Then click on ``Continue to Launch``

    .. image:: resources/trial_to_launch.png
        :alt: to launch
        :align: center

5. Click on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

    .. image:: resources/trial_container_images.png
        :alt: container images
        :align: center

    Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

    .. code-block::
        
        aws ecr get-login-password \
            --region us-east-1 | docker login \
            --username AWS \
            --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-ecs:tech_trialv0.0.4,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-ecs:cluster1_trialv0.0.4,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-ecs:cluster2_trialv0.0.4,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-ecs:flow_trialv0.0.4"    

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the model container images is:

    - **Technique container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-ecs:tech_trialv0.0.4``

    - **Cluster Part 1 container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-ecs:cluster1_trialv0.0.4``

    - **Cluster Part 2 container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-ecs:cluster2_trialv0.0.4``

    - **Flow container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-ecs:flow_trialv0.0.4``


Cypienta User Interface (UI)
----------------------------

1. Subscribe to the `Cypienta User Interface (UI) <https://aws.amazon.com/marketplace/pp/prodview-s4qek5tyez6zk>`__ by clicking on ``Continue to Subscribe`` button.

    .. image:: resources/ui_product.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

    .. image:: resources/confirm_subscribe.png
        :alt: confirm subscribe
        :align: center

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``v0.5``. Then click on ``Continue to Launch``

    .. image:: resources/to_launch.png
        :alt: to launch
        :align: center

5. Click on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

    .. image:: resources/container_images.png
        :alt: container images
        :align: center

    Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

    .. code-block::
        
        aws ecr get-login-password \
            --region us-east-1 | docker login \
            --username AWS \
            --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:nginx-marketv0.0.3,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:marketv0.5"    

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the two images are:

    - **Web container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:marketv0.5``
    
    - **Nginx container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:nginx-marketv0.0.3``
