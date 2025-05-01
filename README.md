# Part - 1

Just a quick reminder about the subnets we configured in our VPC in the [Previous project](./AWS VPC mini project.md). In the public subnet, we've created an EC2 instance that is running, hosting our website. Now, let's take a moment to see if we can access the website using its public IP address.

So this EC2 instance hosts our website.


![initialising-instance](Images/Part-1.1-Initiating_instance.png)


Here's the security group configuration for the instance. In the inbound rules, only IPv4 SSH traffic on port 22 is permitted to access this instance.


![secritygrp-inboundrule](Images/Part1-1b-Securitygrp-inboundrule.png)

For the outbound rule, you'll notice that all IPv4 traffic with any protocol on any port number is allowed, meaning this instance has unrestricted access to anywhere on the internet.


![outbound-rule](Images/Part1-1-outbound-rule.png)

Now, let's test accessibility to the website using the public IP address assigned to this instance.
Here, let's retrieve the public IP address.

![testing-ip](Images/Part-1-1-testing_theIP.png)


If you enter "(https://13.60.201.145/)" into your Chrome browser, and hit enter, you'll notice that the page doesn't load; it keeps attempting to connect. And finally it'll show this page. After some time, you'll likely see a page indicating that the site can't be reached.


![outoome-image](<Images/Part1-1-outcomes-of ip.png>)


This is because of the security group, because we haven't defined HTTP protocol in the security group so whenever the outside world is trying to go inside our instance and trying to get the data, security group is restricting it and that's why we are unable to see the data.

To resolve this issue, we can create a new security group that allows HTTP (port 80) traffic.
1. Navigate to the "Security Groups" section on the left sidebar.

a) Then click on "Create Security Group".

![security-grp-creation](Images/Part1-1a_securitygrp-creation.png)


2. Please provide a name and description for the new security group.
a) Ensure to select your VPC during the creation process.

![security-grp-creation](Images/Part1-2a-securitygrp-creation.png)

b) Click on add rule.

![adding-inbound](Images/Part2-2a-adding-inbound-rule.png)



c) Now, select "HTTP" as the type.

![HTTP-selection](Images/Part1-2c-inbound-selection.png)

d) Use 0.0.0.0/0 as the CIDR Block. (Here we are allowing every CIDR block by using this CIDR).
Now you will see the rule have been created.

![CIDR-selection](Images/part1-2d-CIDR-SELECTION.png)

e) Keep outbound rules as it is.

![outbound-rule](Images/Part1-2e-outbound-rule.png)

f) Now, click on Create security group.

![creation-security](Images/Part1-1f-create-securitygrp.png)


Now, it is being created successfully.


![successful-inbound-creation](Images/part1-1fb-successful-creation-inbound-rule.png)


Let's attach this security group to our instance.

3. Now navigate to the instance section of left side bar.

a) Select the instance.

b) Click on "Actions."

c) Choose "security.

![creating-instance](Images/part1-3(a-c)-creating-instance-secutirty.png)

d) Click on "Change security group."

![securitygrp-creation](Images/Part1-3(d)-securitygrp-creation.png)


4. Choose the security group you created.

![security-grpselection](Images/part1-4-securitygrp-selection.png)

b) You can see security group is being added, Click on "save."

Note - The security group named "Launch Wizard" you see is the default security group automatically attached when creating the instance. You can also edit this security group if needed.

![created-grp-selection](Images/Part1-4b.png)

5. Now it is being attached successfully,

a) If you again copy the public IP address,

![successful-creatd-secgrp](Images/part1-5-successful-created-secgrp.png)

b) And write  http://13.60.201.145 in Chrome, We'll be able to see the data of our website.

![confirmation](Images/Part1-5b-confirmation.png)

Currently, let's take a look at how our inbound and outbound rules are configured.

This setup allows the HTTP and SSH protocols to access the instance.

![5c-confirmation](Images/part1-5c.png)

The outbound rule permits all traffic to exit the instance.

![part5d-outbound-confirmation](Images/part1-5d.png)

Through this rule, we're able to access the website.

![website-confirmation](Images/Part1-5b-confirmation.png)


6. let's see how removing the outbound rule affects the instance's connectivity. Means now, no one can go outside to this instance.

a) Go to outbound tab.

b) Click on "edit outbound rules".

![editing-outbound-rule](Images/Part1-6-editing-outbound-rule.png)

c) Click on "Delete."

d) Click on "Save rules."

![deleting-outbound-rule](Images/part-6-c&d-deleting-outbound-rule.png)

Now that we've removed the outbound rule, let's take a look at how it appears in the configuration.


![deleted-outboundrule](Images/Part1-6-6d-deletion-of-outboundrule.png)


After making this change, let's test whether we can still access the website.

![confirmed-site](Images/Part1-5b-confirmation.png)


So, even though we've removed the outbound rule that allows all traffic from the instance to the outside world, we can still access the website. According to the logic we discussed, when a user accesses the instance, the inbound rule permits HTTP protocol traffic to enter. However, when the instance sends data to the user's browser to display the website, the outbound rule should prevent it. Yet, we're still able to view the website. Why might that be?

Security groups are stateful, which means they automatically allow return traffic initiated by the instances to which they are attached. So, even though we removed the outbound rule, the security group allows the return traffic necessary for displaying the website, hence we can still access it.

let's explore the scenario,

If we delete both the inbound and outbound rules, essentially, we're closing all access to and from the instance. This means no traffic can come into the instance, and the instance cannot send any traffic out. So, if we attempt to access the website from a browser or any other client, it will fail because there are no rules permitting traffic to reach the instance. Similarly, the instance won't be able to communicate with any external services or websites because all outbound traffic is also blocked.

7. You will be able to delete the inbound rule in the same way we have deleted the outbound rule."

a) Go to outbound tab.

b) Click on edit inbound rule

![editing-inbound](Images/part1-7(a-b)-editing-inbound.png)


C) Click on delete,
 
d) Click on "Save rule."

![deleting-inbound](Images/part1-7(c&d).png)


Currently, let's have a look at how our inbound and outbound rules are configured.

![inbound-status](Images/part1-7-inbound-status.png)

![outbound-status](Images/part1-7-outbound-status.png)

Now, as both the inbound and outbound rules deleted, there's no way for traffic to enter or leave the instance. This means that any attempt to access the website from a browser or any other client will fail because there are no rules permitting traffic to reach the instance. In this state, the instance is essentially isolated from both incoming and outgoing traffic.

So you can't access the website now.

![website-unaccessible](<Images/part1-7e-inaccessible site.png>)


In the next scenario,

We'll add a rule specifically allowing HTTP traffic in the outbound rules. This change will enable the instance to initiate outgoing connections over HTTP.

8. Click on edit outbound rule in the outbound tab,

![editing-outbound-rule](Images/part1-8-editing-outbound-rule.png)


a) Click on "add rule"

b) Choose type.

c) Choose destination.

d) Choose CIDR.

e) Click on "save rules"

![editing-outbound-rule](Images/part1-8(b-d)-creation-outbound-rule.png)

![new-outbound-rule-done](Images/part1-8-outbound-rule-done.png)

![inbound-status](Images/part1-8-f-inbound-rule.png)

Now, let's see if we can access the website,

![site-unaccessible](<Images/part1-7e-inaccessible site.png>)


So, we are not able to see it.

But if you look here, we are able to go to the outside world from the instance. We are using here.

![different-access-route](Images/part1-8f-acessing-from-different-route.png)


Note- curl is a command-line tool that fetches data from a URL.
As a result, the instance will be able to fetch data from external sources or communicate with other HTTP-based services on the internet. This adjustment ensures that while incoming connections to the instance may still be restricted, the instance itself can actively communicate over HTTP to external services.

# Part - 2

Let's come to NACL,

1. First navigate to the search bar and search for VPC.
a) Then click on VPC,

![NACL-creation](Images/part2-1-VPCcreation.png)

2. Navigate to the Network ACLs in the left sidebar.

a) Click on "Create Network ACL."

![NACL](Images/part2-2-NACL-creation.png)

3. Now, provide a name for your Network ACL,

a) Choose the VPC you created in the [Previous session](./AWS VPC mini project.md) for the practical on VPC creation,

b) Then click on "Create network ACL".

![new-ACL-created](Images/Part2-2b-NACLcreated.png)

4. If you selected the Network ACL you created,
a) navigate to the "Inbound" tab.

By default, you'll notice that it's denying all traffic from all ports.

![inbound-traffic](Images/Part2-4-editing-inbound-rule.png)

Similarly, if you look at the outbound rules, you'll observe that it's denying all outbound traffic on all ports by default.

b) Select the NACL.

c) And navigate to the "Outbound" tab.


![outbound-traffic](Images/Part2-4b-editing-outbound-rule.png)


5. To make changes,

a) select the NACL,

b) Go to the "Inbound" tab.

c) And click on "Edit inbound rules".

![inbound-rule-edited](Images/Part5-5a-editing-inbound-rule.png)


6. Now, click on "Add new rule."

![add-new-rule](Images/Part2-6-adding-new-rule.png)

7. Now, choose the rule number.

a) Specify the type.

b) Select the source.

c) And determine whether to allow or deny the traffic.

d) Then click on "Save changes."

![new-ruleadded](Images/Part2-7-adding-new-rule.png)

Currently, this NACL is not associated with any of the subnets in the VPC.

![unassociated-vpc](Images/Part2-7b.png)

8. Let's associate it.

a) Select your NACL.

b) Click on "Actions."

c) Choose "Edit subnet association."

![editing-subnet](Images/Part2-8-editing-subnet.png)


d) Then select your public subnet, as our instance resides in the public subnet.

![attahing-public-subnet](Images/part2-8c-attaching-subnet.png)

You have successfully associated your public subnet to this NACL.

![successful attachment](Images/Part2-7d-successful-attachment.png)


As soon as you have attached this NACL to your public subnet, and then you try to access the website again by typing the URL http://10.0.0.0/18/, you will
notice that you are unable to see the website.

![failed-conncetion](Images/part2-8d.png)


Although we've permitted all traffic in the inbound rule of our NACL, we're still unable to access the website. This raises the question: why isn't the website visible despite these permissions?

The reason why we're unable to access the website despite permitting inbound traffic in the NACL is because NACLs are stateless. They don't automatically allow return traffic. As a result, we must explicitly configure rules for both inbound and outbound traffic.

Even though the inbound rule allows all traffic into the subnet, the outbound rules are still denying all traffic.

You can see,

![inbound-traffic-display](Images/Part2.8.e.png)
![outbound-traffic](Images/Part-2-8e.b.png)

9. If we allow outbound traffic as well,

a) Choose you NACL.
b) Go to outbound tab.
c) Click on "Edit outbound rules."

![editing-outbpund-rule](<Images/Part-2-9.(a-c).editing outboud.png>)

d) Click on "Add rule."

![adding-new-rule](Images/Part-2.9d.adding-outbound-rule.png)

You have successfully created the rules,

![successful-created-rule](Images/Part2-9b-d.png)

Upon revisiting the website, you should now be able to access it without any issues.


![site-accessible](Images/Part2-9-e.png)

Now, let's see one more interesting scenario,

In this scenario:

Security Group: Allows inbound traffic for HTTP and SSH protocols and permits all outbound traffic.

Network ACL: Denies all inbound traffic. Let's observe the outcome of this configuration.
Security group,
Configuring it,

![configuring-inbound-rule](Images/Part-2-9e-f.png)

![inbound-rule-display](Images/Part2.9.fb.png)


NACL,

Let's remove it so by default it be denied all traffic.

![part2-9-inbound-rule-removed](Images/Part2.gi-inbound-rule-removed.png)

Additionally, the outbound rule will be removed, defaulting to deny all traffic by default.

![part2-9-outbound-rule-removal](Images/Part2.9hi.png)

Now, let's try to access the website,

![part2-9-outbound-rule-removal](Images/Part2.9hi.png)