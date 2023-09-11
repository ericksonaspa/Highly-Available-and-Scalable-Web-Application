# Highly-Available-and-Scalable-Web-Application
Highly Available and Scalable Web Application

We will build a highly available web application on two availability zones and will perform a stress test to scale from the desired to the maximum capacity. Let's get started! 

# Create VPC

Amazon Virtual Private Cloud (Amazon VPC) allows you to start AWS resources with a user-defined virtual network. This virtual network, along with the benefits of using AWS's scalable infrastructure, is very similar to the existing network operating in the customer's own data center.

1. On the AWS console, select **VPC** from the service menu. Open it in a new tab.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/ea7c491c-e173-4e15-866e-a3e706ec8a1a)

2. Select **VPC Dashboard** and click **Create VPC** to create your own VPC.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/cb7d19ce-877b-422a-957d-98a1c1ed4ae1)

3. To create a space to provision AWS resources used in this lab, we will create a VPC and Subnets. Select **VPC and more** in Resource to create tab and change name tag to **Project-VPC-Lab**. Leave the default setting for IPv4 CIDR block.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/3d551d0e-9a81-40d6-b2e1-ab30122492e2)

4. To design high availability architecture, we create **2** subnet space and select **2a** and **2b** for **Customize AZs**. And set the CIDR value of the public subnet that can communicate directly with the Internet as shown in the screen below. Set the CIDR value of the private subnet as

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/d6ae8042-2d1f-4917-a094-ce8f69123b39)

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/b85eb999-f7b7-4378-93cf-22832ff6c6bd)

5. You can use a **NAT gateway** so that instances in your private subnets can connect to services outside your VPC, but external services cannot initiate direct connections to these instances. In this lab, we will create a NAT gateway in only one Availability Zone to save cost. Also, for DNS options, **enable** both **DNS hostnames** and **DNS resolution**. After confirming the setting value, click the **Create VPC** button.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/707be89f-a800-4cbd-b505-50d8a3b4c5b0)

6. As the VPC is created, you can see the process of creating network-related resources as shown in the screen below. For NAT Gateway, provisioning may take longer compared to other resources.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/894da9d3-4f0c-434e-a8a3-82e28a33687e)

# Launch a Web Server Instance

1. First, let's create our Web Server instance. We will launch an Amazon Linux 2 instance, bootstrap Apache/PHP, and install a basic web page that will display information about our instance. Click on **EC2 Dashboard** near the top of the leftmost menu. And Click on **Launch instances**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/75ec96d3-defb-4fb2-b0f0-e7ea137c8e58)

2. In Name, put the value **Web server**. And check the default setting for Amazon Machine Image below.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/f3301122-1ff7-4122-bc4d-99c1f5b9f1ff)

3. Select **t2.micro** in **Instance Type**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/953da83d-73d5-4ca7-9bc7-c2dab1071a23)

4. Select the **Proceed without a key pair (Not recommended)**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/e5199468-29e8-499c-aebd-72a3b257c2d9)

6. Click the **Edit** button in **Network settings** to set the space where EC2 will be located.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/d396f0df-c7e9-4b02-bfce-47722438c066)

6. Check default VPC and subnet. **Auto-assign public IP** is set to **Enable**. 

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/47099bae-5b41-4481-9eff-94d7edb29f1a)

7. Right below it, create Security groups to act as a network firewall. Security groups will specify the protocols and addresses you want to allow in your firewall policy. For the security group you are currently creating, this is the rule that applies to the EC2 that will be created. After entering **Project - Web Server** in **Security group name** and **Description**, select **Add Security group rule** and set **HTTP** to **Type**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/d7d03818-4d58-421b-870b-c329ff4c69a9)

Select **my IP** for both **HTTP** and **SSH** traffic as the **Source type**. 

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/a39dd22f-19b6-48f2-89cf-9dab09512551)

8. All other values accept the default values, expand by clicking on the **Advanced Details** tab at the bottom of the screen.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/2d81123c-0ed4-4cf7-82f2-5b59eb6ed71a)

9. Enter the following values in the User data field and select Launch instance.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/f4e2c277-9fd0-4725-9cff-01adf7c2a2e3)

10. Click the **View Instances** button in the lower right-hand portion of the screen to view the list of EC2 instances. Once your instance has launched, you will see your Web Server as well as the Availability Zone the instance is in, and the publicly routable DNS name. Click the checkbox next to your web server to view details about this EC2 instance.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/3d2d29cd-a742-409c-bef1-1a31330aba84)

11. Wait for the instance to pass the Status Checks to finish loading. Open a new browser tab and browse the Web Server by entering the EC2 instance’s Public DNS name into the browser. The EC2 instance’s Public DNS name can be found in the console by reviewing the Public IPv4 DNS name line highlighted above. You should see a website that looks like the following.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/de7ef5a0-6a7d-471d-babf-b9a52b6b1766)

# Connect to your Linux instance using Session Manager

Session Manager is a fully managed AWS Systems Manager capability that lets you manage your Amazon EC2 instances through an interactive one-click browser-based shell or through the AWS CLI. You can use Session Manager to start a session with an instance in your account. After the session is started, you can run bash commands as you would through any other connection type.

1. On the AWS Management Console, search for the IAM console and open it in a new tab. In the navigation pane, choose **Roles**, and then choose **Create role**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/4f65edb4-5e95-46db-b4a8-dcde291c3069)

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/2b825fb9-0d02-4f94-8de4-3b8cb788bac2)

2. Under **Select type of trusted entity**, choose **AWS service**. Immediately under **Choose the service that will use this role**, choose **EC2**, and then choose **Next**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/6626e733-2e79-42f1-b54e-e291aac5f97f)

3. On the **Attach permissions** policies page, do the following: Use the **Search** field to locate the **AmazonSSMManagedInstanceCore**. Select the box next to its name. Choose **Next**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/86a70df9-3d71-4c53-9ba9-dca737e0438e)

4. For Role name, enter a name for your new instance profile, such as **SSMInstanceProfile**. Choose **Create role**. The system returns you to the **Roles** page. 

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/897bd207-1a10-4bd7-9cde-69f0a0239ff0)

5. Go back Amazon EC2 console. In the navigation pane, under **Instances**, choose **Instances**. Choose your EC2 instance from the list and click **Actions**. In the Actions menu, choose Security, Modify IAM role.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/eeeea30d-ee77-4c79-a37f-3e81057f8e1b)

6. For **IAM** role, select the instance profile you created **SSMInstanceProfile**. Then click on **Update IAM role**. 

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/edadc126-8e07-4935-8ad9-4bab24e233a4)

7. In the EC2 instance console, select the instance you want to connect to, and then click the **Connect** button.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/e599ddcd-2efd-453a-8fde-16f15becff74)

8. In the **Connect to instance** page, select **Session Manager**. Review the **Session Manager usage section** for advantages of using Session Manager. Choose Connect. A new session will be started in a new tab. After the session is started, you can run bash commands as you would through any other connection type.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/f8b0803c-d3dd-4898-a7e1-4d0f5e5ada84)

# Create a custom AMI

In the AWS EC2 console, you can create an Custom AMI to meet your needs. This can then be used for future EC2 instance creation. In this page, let's create an AMI using the web server instance that we built earlier.

1. In the EC2 console, select the instance that we made earlier in this lab, and click **Actions > Image and templates > Create Image**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/d48e2087-d1e8-4d4d-b401-0f563ed3690a)

2. In the Create Image console, type as shown below and press Create image to create the custom image.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/3023745a-9f8f-418c-95cf-1166b9ce949e)

3. Verify in the console that the image creation request in completed. In the left navigation panel, Click the **AMIs** button located under **IMAGES**. You can see that the **Status** of the AMI that you just created. It will show either **Pending** or **Available**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/b025cc5b-d7ba-4ed4-9235-4ced326efb39)

# Deploy auto scaling web service

We will deploy a web service that can automatically scale out/in under load and ensure high availability. We use the web server AMI created in the previous chapter and the network infrastructure named **Project-VPC-Lab**.

1. From the **EC2 Management Console** in the left navigation panel, click **Load Balancers** under **Load Balancing**. Then click **Create Load Balancer**. In the Select load balancer type, click the **Create** button under **Application Load Balancer**.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/9907b3c4-c625-467c-8e47-249865ffb49d)

2. Name the load balancer. In this case, name Name as **Web-ALB**. Leave the other settings at their default values.

![image](https://github.com/ericksonaspa/Highly-Available-and-Scalable-Web-Application/assets/77118362/8227ed2d-9eb0-4a4b-9b08-16fee351a59d)

3. Scrolling down a little bit, there is a section for selecting availability zones. First, Select our vpc created previously. For Availability Zones select the 2 public subnets that were created previously. This should be Public Subnet for ap-northeast-2a and Public Subnet C for ap-northeast-2c.



