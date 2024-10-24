Subscribing to Cypienta products on AWS Marketplace
===================================================

ATTACK Technique Detector
-------------------------

1. Subscribe to the `Cypienta ATTACK Technique Detector <https://aws.amazon.com/marketplace/pp/prodview-dad2myycdjz2s>`__ by clicking on ``Continue to Subscribe`` button.

    .. image:: resources/technique_subscribe.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

    .. image:: resources/technique_confirm.png
        :alt: confirm subscribe
        :align: center

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``v0.9``. Then click on ``Continue to Launch``

    .. image:: resources/technique_to_launch.png
        :alt: to launch
        :align: center

5. Click on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

    .. image:: resources/technique_container_images.png
        :alt: container images
        :align: center

    Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

    .. code-block::
        
        aws ecr get-login-password \
            --region us-east-1 | docker login \
            --username AWS \
            --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/technique-detector:v0.9.1"    

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the model container images is:

    - **Technique container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/technique-detector:v0.9.1``


Temporal Clustering Part-1
-------------------------

1. Subscribe to the `Cypienta Temporal Clustering Part-1 <https://aws.amazon.com/marketplace/pp/prodview-y2srnvk75fi3s>`__ by clicking on ``Continue to Subscribe`` button.

    .. image:: resources/embed_subscribe.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

    .. image:: resources/embed_confirm.png
        :alt: confirm subscribe
        :align: center

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``v0.9``. Then click on ``Continue to Launch``

    .. image:: resources/embed_to_launch.png
        :alt: to launch
        :align: center

5. Click on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

    .. image:: resources/embed_container_images.png
        :alt: container images
        :align: center

    Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

    .. code-block::
        
        aws ecr get-login-password \
            --region us-east-1 | docker login \
            --username AWS \
            --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cluster-part-1:v0.9.1"

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the model container images is:

    - **Cluster part 1 container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cluster-part-1:v0.9.1``


Temporal Clustering Part-2
-------------------------

1. Subscribe to the `Cypienta Temporal Clustering Part-2 <https://aws.amazon.com/marketplace/pp/prodview-yvjpz6yrorld2>`__ by clicking on ``Continue to Subscribe`` button.

    .. image:: resources/cluster_subscribe.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

    .. image:: resources/cluster_confirm.png
        :alt: confirm subscribe
        :align: center

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``v0.9``. Then click on ``Continue to Launch``

    .. image:: resources/cluster_to_launch.png
        :alt: to launch
        :align: center

5. Click on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

    .. image:: resources/cluster_container_images.png
        :alt: container images
        :align: center

    Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

    .. code-block::
        
        aws ecr get-login-password \
            --region us-east-1 | docker login \
            --username AWS \
            --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cluster-part-2:v0.9.1"

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the model container images is:

    - **Cluster part 1 container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cluster-part-2:v0.9.1``


ATTACK Flow Detector
-------------------------

1. Subscribe to the `Cypienta MITRE ATTACK Flow Detector <https://aws.amazon.com/marketplace/pp/prodview-uixbbbwiwb67s>`__ by clicking on ``Continue to Subscribe`` button.

    .. image:: resources/flow_subscribe.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

    .. image:: resources/flow_confirm.png
        :alt: confirm subscribe
        :align: center

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``v0.9``. Then click on ``Continue to Launch``

    .. image:: resources/flow_to_launch.png
        :alt: to launch
        :align: center

5. Click on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

    .. image:: resources/flow_container_images.png
        :alt: container images
        :align: center

    Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

    .. code-block::
        
        aws ecr get-login-password \
            --region us-east-1 | docker login \
            --username AWS \
            --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-detector:v0.9.1"    

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the model container images is:

    - **Flow container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-detector:v0.9.1``


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


ATTACK Flow Detector
-------------------------

1. Subscribe to the `Cypienta MITRE ATTACK Flow Detector <https://aws.amazon.com/marketplace/pp/prodview-uixbbbwiwb67s>`__ by clicking on ``Continue to Subscribe`` button.

    .. image:: resources/flow_subscribe.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

    .. image:: resources/flow_confirm.png
        :alt: confirm subscribe
        :align: center

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``v0.9``. Then click on ``Continue to Launch``

    .. image:: resources/flow_to_launch.png
        :alt: to launch
        :align: center

5. Click on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

    .. image:: resources/flow_container_images.png
        :alt: container images
        :align: center

    Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

    .. code-block::
        
        aws ecr get-login-password \
            --region us-east-1 | docker login \
            --username AWS \
            --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-detector:v0.9.1"    

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the model container images is:

    - **Flow container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-detector:v0.9.1``


ATTACK Flow Detector
-------------------------

1. Subscribe to the `Cypienta MITRE ATTACK Flow Detector <https://aws.amazon.com/marketplace/pp/prodview-uixbbbwiwb67s>`__ by clicking on ``Continue to Subscribe`` button.

    .. image:: resources/flow_subscribe.png
        :alt: UI product subscribe
        :align: center

2. On the next page, click on ``Accept terms`` button to agree with the terms.

3. Wait for the subscription confirmation page to appear. Then click on ``Continue to Configuration``.

    .. image:: resources/flow_confirm.png
        :alt: confirm subscribe
        :align: center

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``v0.9``. Then click on ``Continue to Launch``

    .. image:: resources/flow_to_launch.png
        :alt: to launch
        :align: center

5. Click on the Copy button in the ``Container images`` section and make note of the ``CONTAINER_IMAGES``

    .. image:: resources/flow_container_images.png
        :alt: container images
        :align: center

    Make note of the ``CONTAINER_IMAGES`` from the copied snippet:

    .. code-block::
        
        aws ecr get-login-password \
            --region us-east-1 | docker login \
            --username AWS \
            --password-stdin 709825985650.dkr.ecr.us-east-1.amazonaws.com
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-detector:v0.9.1"    

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the model container images is:

    - **Flow container image:** ``709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/flow-detector:v0.9.1``
