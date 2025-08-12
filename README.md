# Amazon Scaling And Load Balancing Your Architecture

Auto Scaling helps keep your app running smoothly by automatically adding or removing Amazon EC2 servers based on the rules you set. It makes sure you always have the right number of servers running. If more people are using your app, it adds servers to keep things fast. When fewer people are using it, it removes servers to save money. Auto Scaling works well for apps that have steady usage or change at certain times, like during the day or week.

See the Starting Architecture:





<img width="967" height="559" alt="image" src="https://github.com/user-attachments/assets/77a3188d-83be-43a3-bc3d-8cd2af4ef508" />























And the Final Architecture:






<img width="960" height="588" alt="image" src="https://github.com/user-attachments/assets/e9342018-e2f0-4633-a124-3c56a4866f5b" />















Let us following these steps to archive our objective:

**STEP 1: CREATING AN AMI FOR AUTO SCALING**


Create an AMI from the existing Web Server 1. This action saves the contents of the boot disk so that new instances can be launched with identical content.

1.1 On the **AWS Management Console**, in the **Search** bar, enter and choose EC2 to open the **Amazon EC2 Management Console** > In the left navigation pane, locate the **Instances** section, and choose **Instances**.

_The Web Server 1 instance is listed. You now create an AMI based on this instance._

1.2 





<img width="892" height="157" alt="image" src="https://github.com/user-attachments/assets/be75723d-ca1e-4e46-86ae-324ab388a651" />








<img width="1570" height="480" alt="image" src="https://github.com/user-attachments/assets/244bbb42-e877-4964-b577-1362f0ea7b0d" />






1.3 Choose **Create image**







<img width="1573" height="364" alt="image" src="https://github.com/user-attachments/assets/28077320-d49d-4ed3-9337-78762109cb8e" />






_After then you will see a confirmation screen displays the AMI ID for your new AMI. You use this AMI when launching the Auto Scaling group later in the lab._


**STEP 2: CREATING A LOAD BALANCER**

Create a load balancer that can balance traffic across multiple EC2 instances and Availability Zones.

2.1 In the left navigation pane, locate the **Load Balancing** section, and choose Load Balancers > Choose **Create load balancer**.

2.2 In the **Load balancer types** section, for **Application Load Balancer**, choose **Create**.

2.3 On the Create Application Load Balancer page, in the Basic configuration section, configure the following option:

  * For the Load balancer name, enter _LabELB_

    

<img width="1568" height="635" alt="image" src="https://github.com/user-attachments/assets/eb092a6a-8ee1-4d75-a98c-b9f338398698" />




2.4 In the **Network mapping** section, configure the following options:

  * For **VPC**, choose **Lab VPC**.

  * For **Mappings**, choose both Availability Zones listed.

  * For the first Availability Zone, choose **Public Subnet 1**.

  * For the second Availability Zone, choose **Public Subnet 2**.


_These options configure the load balancer to operate across multiple Availability Zones._




<img width="1544" height="625" alt="image" src="https://github.com/user-attachments/assets/9bb264f4-a296-48a4-96fc-48b3fc97842f" />





2.5 In the **Security groups** section, choose the **X** for the **default** security group to remove it.



<img width="650" height="303" alt="image" src="https://github.com/user-attachments/assets/4bf73702-24d2-4148-bd33-766c007a64b6" />



2.6 From the **Security groups** dropdown list, choose **Web Security Group**.




<img width="645" height="301" alt="image" src="https://github.com/user-attachments/assets/3a010453-7e25-4000-8efa-21e1b7cb2617" />




_The Web Security Group has already been created for you, which permits HTTP access._



2.7 In the **Listeners and routing** section, choose the **Create target group** link.

_**Note**: This link opens a new browser tab with the **Create target group** configuration options._

2.8 On the new **Target groups** browser tab, in the **Basic configuration** section, configure the following:

  * For **Choose a target type**, choose **Instances**.

  * For **Target group name**, enter _lab-target-group_

2.9 At the bottom of the page, choose **Next**. > On the **Register targets** page, choose **Create target group**.


_Once the target group has been created successfully, close the **Target groups** browser tab._




<img width="1574" height="544" alt="image" src="https://github.com/user-attachments/assets/e2f6844b-90f3-466a-bf76-49241d329e48" />





2.10 Return to the **Load balancers** browser tab. In the **Listeners and routing** section, choose **Refresh Icon** to the right of the **Forward to** dropdown list for **Default action**.

2.11 From the **Forward to** dropdown list, choose **lab-target-group** > From the Forward to dropdown list, choose lab-target-group.





<img width="644" height="571" alt="image" src="https://github.com/user-attachments/assets/b85e1905-2afd-40f8-8c1e-2ecfa7216f94" />





2.12 At the bottom of the page, choose **Create load balancer**.

  You should receive a message similar to the following:

  * Successfully created load balancer: LabELB




<img width="1572" height="670" alt="image" src="https://github.com/user-attachments/assets/556342d6-6e06-48e4-9fbd-87bca058e645" />




2.13  To view the **LabELB** load balancer that you created, choose **View load balancer**.

2.14 To copy the **DNS name** of the load balancer, use the copy option , and paste the DNS name into a text editor/Notepad

  * You need this information later in the lab.

**STEP 3: CREATING A LAUNCH TEMPLATE**

On this step you create a **launch template** for your Auto Scaling group. A launch template is a template that an Auto Scaling group uses to launch EC2 instances. When you create a launch template, you specify information for the instances, such as the AMI, instance type, key pair, security group, and disks.

3.1 At the top of the AWS Management Console, in the search bar, enter and choose _EC2_ > In the left navigation pane, locate the **Instances** section, and choose **Launch Templates** > Choose **Create launch template**.

3.2 On the **Create launch template** page, in the **Launch template name and description** section, configure the following options:

 * For **Launch template name - _required_**, enter _lab-app-launch-template_

 * For **Template version description**, enter _A web server for the load test app_




<img width="1037" height="560" alt="image" src="https://github.com/user-attachments/assets/d64df3f6-47ea-45e9-9834-1103e764eca4" />





 * For **Auto Scaling guidance**, choose **Provide guidance to help me set up a template that I can use with EC2 Auto Scaling.**





<img width="613" height="161" alt="image" src="https://github.com/user-attachments/assets/774b9cf6-cd59-4f61-8b86-9f5ec2ef6066" />





3.3 In the **Application and OS Images (Amazon Machine Image) - required** section, choose the **My AMIs** tab. Notice that **Web Server AMI** is already chosen.





<img width="1030" height="535" alt="image" src="https://github.com/user-attachments/assets/1527652d-2f2b-457c-a1c8-cfacf1e1846a" />





3.4 In the **Instance type** section, choose the **Instance type** dropdown list, and choose **t3.micro**.

3.5 In the **Key pair (login)** section, confirm that the **Key pair name** dropdown list is set to **Don't include in launch template**.




<img width="476" height="163" alt="image" src="https://github.com/user-attachments/assets/b2151bba-1f99-41c6-a874-a73845ab3858" />



3.6 In the **Network settings** section, choose the **Security groups** dropdown list, and choose **Web Security Group**.




<img width="863" height="482" alt="image" src="https://github.com/user-attachments/assets/8423877a-dabb-4ff2-8cab-c71f2f4f0325" />





_When you launch an instance, you can pass user data to the instance. The data can be used to run configuration tasks and scripts._


3.7 Choose **Create launch template**.

  * You should receive a message similar to the following:

  * Successfully created lab-app-launch-template.




<img width="948" height="773" alt="image" src="https://github.com/user-attachments/assets/d1b96e91-b521-45ef-a094-ebc67fd5c99a" />





3.8 Choose **View launch templates**.




<img width="1899" height="300" alt="image" src="https://github.com/user-attachments/assets/b38907eb-d487-43bc-82a5-a777625fa3c5" />




**STEP 4: CREATING AN AUTO SCALING GROUP**


4.1 <img width="911" height="30" alt="image" src="https://github.com/user-attachments/assets/7fd53a6f-31cc-41c0-90af-293e26224197" />

4.2 On the **Choose launch template or configuration** page, in the Name section, for Auto Scaling group name, enter _Lab Auto Scaling Group_

4.3 Choose **Next**

4.4 On the Choose instance launch options page, in the Network section, configure the following options:

   * From the VPC dropdown list, choose Lab VPC.

   * From the Availability Zones and subnets dropdown list, choose **Private Subnet 1 (10.0.1.0/24)** and **Private Subnet 2 (10.0.3.0/24)**.

4.5 On the Configure advanced options – optional page, configure the following options: 

   * In the Load balancing – optional section, choose Attach to an existing load balancer.

   * In the Attach to an existing load balancer section, configure the following options:

   * Choose Choose from your load balancer target groups.

   * From the **Existing load balancer target groups** dropdown list, choose **lab-target-group | HTTP**.

In the **Health checks – optional** section, **for Health check type**, choose  **ELB**.

4.6 Choose **Next**

4.7 On the **Configure group size and scaling policies – optional** page, configure the following options: 

   * In the Group size – optional section, enter the following values: 

   * Desired capacity:2

   * Minimum capacity: 2

   * Maximum capacity: 4

In the Scaling policies – optional section, configure the following options:

  * Choose  Target tracking scaling policy.

  * For Metric type, choose Average CPU utilization.

  * Change the Target value to 50

This change tells Auto Scaling to maintain an average CPU utilization across all instances of 50 percent. Auto Scaling automatically adds or removes capacity as required to keep the metric at or close to the specified target value. It adjusts to fluctuations in the metric due to a fluctuating load pattern.


4.8 Choose **Next**

4.9 On the Add notifications – optional page, choose Next.

  * On the Add tags – optional page, choose Add tag and configure the following options:

  * Key: Enter Name

  * Value - optional: Enter Lab Instance

  * Choose Next.
  * Choose Create Auto Scaling group.

These options launch EC2 instances in private subnets across both Availability Zones.

Your Auto Scaling group initially shows an Instances count of zero, but new instances will be launched to reach the desired count of two instances.

Note: If you experience an error related to the t3.micro instance type not being available, then rerun this task by choosing the t2.micro instance type instead.

**STEP 5: VERIFYING THAT LOAD BALANCING IS WORKING**

5.1 In the left navigation pane, locate the **Instances** section, and choose **Instances**.

You should see two new instances named Lab Instance. These instances were launched by auto scaling.  If the instances or names are not displayed, wait 30 seconds, and then choose refresh .

First, you confirm that the new instances have passed their health check.

5.2 In the left navigation pane, in the Load Balancing section, choose Target Groups.

5.3 Choose lab-target-group.

In the Registered targets section, two Lab Instance targets should be listed for this target group.

5.4 Wait until the Health status of both instances changes to healthy. To check for updates, choose refresh .

   A healthy status indicates that an instance has passed the load balancer's health check. This check means that the load balancer will send traffic to the instance.

   You can now access the instances launched in the Auto Scaling group using the load balancer.
 5.5 Open a new web browser tab, paste the DNS name that you copied before, and press Enter.

   The Load Test application should appear in your browser, which means that the load balancer received the request, sent it to one of the EC2 instances, and then passed back the result.
   
**STEP 6: TESTING AUTO SCALING**

6.1 Return to the AWS Management Console, but keep the Load Test application tab open. You return to this tab soon.

6.2 In the AWS Management Console, in the search bar, enter and choose _CloudWatch_

6.3 In the left navigation pane, in the Alarms section, choose All alarms.

Two alarms are displayed. The Auto Scaling group automatically created these two alarms. These alarms automatically keep the average CPU load close to 50 percent while also staying within the limitation of having 2–4 instances.

6.4 Choose the alarm that has **AlarmHigh** in its name. This alarm should have a **State** of _OK_.

   * If the alarm is not showing OK for the State, wait a minute and then choose refresh  until the State changes.

   The OK state indicates that the alarm has not been initiated. It is the alarm for **CPU Utilization > 50**, which adds instances when the average CPU utilization is high. The chart should show very low levels of CPU at the moment. 

You now tell the application to perform calculations that should raise the CPU level.

6.5 Return to the browser tab with the **Load Test** application.

6.6 Next to the AWS logo, choose **Load Test**.

This step causes the application to generate high loads. The browser page automatically refreshes so that all instances in the Auto Scaling group will generate loads. Do not close this tab.

6.7 Return to browser tab with the CloudWatch Management Console.

  * In less than 5 minutes, the AlarmLow alarm status should change to OK, and the AlarmHigh alarm status should change to In alarm.

   * To update the display, choose refresh  every 60 seconds.

   * You should see the AlarmHigh chart indicating an increasing CPU percentage. Once it crosses the 50 percent line for more than 3 minutes, it initiates auto scaling to add additional instances.

6.8 Wait until the **AlarmHigh** alarm enters the In alarm state.

  You can now view the additional instance or instances that were launched.

6.9 In the AWS Management Console, in the search bar, enter and choose _EC2 _
6.10 In the left navigation pane, locate the **Instances** section, and choose **Instances**.

More than two instances named **Lab Instance** should now be running. Auto scaling created the new instances in response to the alarm.

**STEP 7: TERMINATING THE WEB SERVER 1 INSTANCE**

In the left navigation pane, locate the Instances section, and choose Instances.

More than two instances named Lab Instance should now be running. Auto scaling created the new instances in response to the alarm. 

7.1  <img width="442" height="150" alt="image" src="https://github.com/user-attachments/assets/608b90f3-f18d-4e86-a234-a2890379953a" />



# WELL DONE!!

You have done the following successfully:
Created an AMI from an EC2 instance.

* Created a load balancer.

* Created a launch template and an Auto Scaling group.

* Configured an Auto Scaling group to scale new instances within private subnets.

* Used CloudWatch alarms to monitor the performance of your infrastructure.



