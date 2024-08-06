Subscribing to Cypienta products on AWS Marketplace
===================================================

ATTACK Technique Detector
-------------------------

1. Use the `link <https://aws.amazon.com/marketplace/pp/prodview-ygn2hithg564w?sr=0-2&ref_=beagle&applicationId=AWSMPContessa>`_ to explore the marketplace model packages in AWS. Search for ``Cypienta ATTACK Technique Detector``

    Click on ``Continue to Subscribe``.

    .. image:: resources/subscribe_to_technique_detector.png
        :alt: Subscribe to technique detector
        :align: center

2. Click on the ``Accept offer`` button on the next page.

    .. image:: resources/accept_offer.png
        :alt: Subscribe to technique detector
        :align: center

3. Click on ``Continue to configuration``. In the section ``Select your launch method``, select ``AWS CloudFormation``. Select the ``Software Version`` as ``0.4`` from the drop down. Select the ``Region`` in which you would want to deploy Cypienta products. Copy and make note of the ``Product Arn``.

    .. image:: resources/model_arn_tech.png
        :alt: Subscribe to technique detector
        :align: center


Temporal Clustering
-------------------

1. Use the `link <https://aws.amazon.com/marketplace/pp/prodview-a6owq2ddgrcrc?sr=0-3&ref_=beagle&applicationId=AWSMPContessa>`_ to explore the marketplace model packages in AWS. Search for ``Cypienta Temporal Clustering``

    Click on ``Continue to Subscribe``.

    .. image:: resources/subscribe_to_temporal_clustering.png
        :alt: Subscribe to temporal clustering
        :align: center

2. Click on the ``Accept offer`` button on the next page.

    .. image:: resources/accept_offer.png
        :alt: Subscribe to technique detector
        :align: center

3. Click on ``Continue to configuration``. In the section ``Select your launch method``, select ``AWS CloudFormation``. Select the ``Software Version`` as ``0.7.1`` from the drop down. Select the ``Region`` in which you would want to deploy Cypienta products. Copy and make note of the ``Product Arn``.

    .. image:: resources/model_arn_cluster.png
        :alt: Subscribe to flow detector
        :align: center


MITRE ATTACK Flow Detector
-------------------

1. Use the `link <https://aws.amazon.com/marketplace/pp/prodview-4dismc5uwx4dk?sr=0-1&ref_=beagle&applicationId=AWSMPContessa>`_ to explore the marketplace model packages in AWS. Search for ``Cypienta MITRE ATTACK Flow Detector``

    Click on ``Continue to Subscribe``.

    .. image:: resources/subscribe_to_flow_detector.png
        :alt: Subscribe to technique detector
        :align: center

2. Click on the ``Accept offer`` button on the next page.

    .. image:: resources/accept_offer.png
        :alt: Subscribe to technique detector
        :align: center

3. Click on ``Continue to configuration``. In the section ``Select your launch method``, select ``AWS CloudFormation``. Select the ``Software Version`` as ``0.7`` from the drop down. Select the ``Region`` in which you would want to deploy Cypienta products. Copy and make note of the ``Product Arn``.

    .. image:: resources/model_arn_flow.png
        :alt: Subscribe to technique detector
        :align: center


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

4. Select the ``Fulfillment option`` as ``ECS``. Select the ``Software version`` as ``v0.2.2``. Then click on ``Continue to Launch``

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
            
        CONTAINER_IMAGES="709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:nginx-marketv0.0.3,709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:marketv0.2.3"    

        for i in $(echo $CONTAINER_IMAGES | sed "s/,/ /g"); do docker pull $i; done

    Here the two images are:

    - **Web container image:** 709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:marketv0.2.3
    
    - **Nginx container image:** 709825985650.dkr.ecr.us-east-1.amazonaws.com/cypienta/cytech:nginx-marketv0.0.3
