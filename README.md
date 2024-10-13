# Creating an autoscaling group 

Auto scaling group are required to ensure high availability, scalability and reliability as well as for cost optimization. This service helps to scale up or scale down depending on traffic and usage of the resources. Below steps were taken to create an autoscaling group and tested to see how it operates.


1. Launch an instance, I used RedHat AMI fro this lab
![](/img/01launch-instance.png)
2. fill in the requirement for the instance 
![](/img/02choose-xter.png)
3. Create a security group andf allow port 22 and port 80 from your IP (although in this project, I opened up the port to all traffic, this is not a good practice)
![](/img/03create-sg.png)
4. Enter the userdata as shown below 
    ```bash
    #!/bin/bash
    sudo yum update -y 
    sudo yum install httpd -y
    sudo systemctl start httpd
    sudo chkconfig httpd on
    ```
5. click the lauch button and create the instance 
![](/img/05launch.png)
6. After the instance state has shown running and health checks are done, under action, click on image and templates and create image 
![](/img/06create-image.png)
7. Give a name to the image and maybe description
![](/img/07name-image.png)
8. Click on create
![](/img/08create.png)
9. After creating this image, you can find it in AMIs under image
![](/img/09view-image.png)
10. Click on launch templates from the side menu
![](/img/10launch-templates.png)
11. Click on create launch template
![](/img/11.click.png)
12.  Give the template a name and description
![](/img/12name-template.png)
13. choose My AMIs and select your AMI from the dropdown 
![](/img/13choose-ami.png)
14. Select an instance type and a key pair for the launch template
![](/img/14choose-setting.png)
15. Select your security group or create a new security group, I selected the one i used earlier on for this lab.
![](/img/15choose-sg.png)
16. click on create launch template
![](/img/16click-create.png)
17. From the side menu, select Auto scaling groups under auto scaling
![](/img/17create-asg.png)
18. Create an auto scaling group
![](/img/18create-it.png)
19. Give a name to your auto scaling groups and under launch template, choose the launch template that you recently created then click next
![](/img/19nameit.png)
20. Select your VPC and choose at least 2 availability zones
![](/img/21availabilityzone.png)
21. Choose load balancer if you have one previously created or attach a new one (I didn't use load balancer for this lab so I selected no load balancer)
![](/img/22No-ELB.png)
22. Click on next, 
![](/img/23next.png)
![](/img/24next.png)
23. Specify the capacities to be used by your auto scaling group, desired, minimum and maximum.
![](/img/24specify-capacitty.png)
24. Choose he scaling behaviours, here I used the target tracking scaling policy using 50% of CPU usage as the target
![](/img/25scaling.png)
25. Click on next and create it
![](/img/26next.png)
![](/img/27next.png)
26. Review the sumamry and create
![](/img/28review-create.png)
27. After the ASG is ready, click on it
![](/img/29the-asg.png)
28. Under activities, check the activity history, the activity history of the desired capacity should be visible here.
![](/img/30autoscaling.png)
29. Under instances tab, some new instances should be seen been spinned up
![](/img/31new-instances.png)
30. Copy the IP address of one of the newly created instances
![](/img/32first-instance.png)
31. ssh into one of the newly created instances
![](/img/33login-ssh.png)
32. check the cpu usage of this instance using the top command 
![](/img/35cpu-usage.png)
33. Run below command on the instance to initiate stress on the cpu.
![](/img/36%20new-usage2.PNG)
34. ssh into the second instance and repeat the same command on the instance. This will make the 2 instances over 90% CPU usage
35. Now check back at the activities history of the ASG and observe the report (the timing depend on what is set as the warmup time for the new instance, in this case, it was 300 secs which is the default)
![](/img/37newinstance.png)
36. Under the instance tab, there si also a new instance just been initiated
![](/img/38creatingnew.png)
37. Stop the stress process by killing the pid of the process
![](/img/39stresskilled2.PNG)
38. After about 25 minutes that the stress has been killed, checking through the activity log, some instances are being terminated due to inactivity 
![](/img/40terminating.png)
