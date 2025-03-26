Security Groups 
===============

Security groups in AWS are used to control the inbound and outbound traffic of the resources.

By defult, the security group allows all IPv4 inbound and outbound traffic. The security group is applied to all resources in the cloudformation stack.

How to modify the security group
-------------------------------

To modify the security group, you need to update the security group in the cloudformation stack. Follow the steps below to modify the security group.

1. Go to the AWS console and search for ``CloudFormation``.

2. Select the stack that you want to modify.

3. Click on the ``Resources`` tab of the stack and search for ``SecurityGroup``. Click on the Physical ID link of the security group, this will open the security group in the EC2 resource console.

4. Click on the ``Security Group ID`` link to open the security group.

5. You can view the inbound and outbound rules of the security group in their respective tabs.

6. To edit the outbound rules, click on the ``Outbound Rules`` tab. Then click on ``Edit outbound rules``. Make the necessary changes to the rules as you desire. You can do the same for the inbound rules by clicking on the ``Inbound Rules`` tab and then clicking on ``Edit inbound rules``.

    .. note::
        The security group is applied to all resources in the cloudformation stack. So keep in mind that the changes you make will affect all resources in the stack. The Cypienta UI and Cypienta Airflow must be able to connect to each other. So, you need to allow the traffic between the two. Cypienta UI uses port 8000, Cypienta Airflow uses port 8080. All the resources in the stack must be able to connect to each other, and they use the same VPC.

        Cypienta UI and Cypienta Airflow requires read and write access to S3 bucket, so you need to allow the traffic from the security group to allow AWS API calls to S3 bucket.

7. Click on the ``Save rules`` button to update the security group. The effect of the changes will be available instantly.
