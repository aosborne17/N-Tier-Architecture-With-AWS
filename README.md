# N-Tier-Architecture-With-AWS


N-Tier Architecture (or Multi-tier Architecture) is all about having all of your different functions within a software
e.g. (data, presentation, processing) physically and logically separated.

N in the name refers to any number from 1

In this example, the application has been divided into three different tiers
all with different functions

![](/images/N-Tier-Architecture-3-Tiers.webp)

## Why implement N-Tier Architecture

- When you work on one section, the changes will not affect the other functions
of the software.

- If there is a problem it is much easier to locate as opposed to traditional
monolithic Architecture

- You can secure each of the three tiers separately using different firewall settings,
this has been implemented in the VPC that we will create soon

- It offers scalability, if we need to add more resources, it can be done per tier without
the need to effect other tiers

- Reusability, we can take a tier from one project and implement it onto another project without
the need to remake the whole tier again, thus saving time

## Creating A VPC to host a two-tier application

- Our Webapp tier will run the application and show the webpage on the browse

- Our database tier will connect with mongoDB to collect some data and show it on our
web browser

- To ensure that our database is more secure, we will create a public subnet to host
our app and a private subnet to host our database, after having downloaded mongoDB
onto our DB we will remove all connections to the outside internet and only allow SSHing
in from a secure Bastion server

- This ensures that our Databse is much more secure than our app


## Common Terminology


- Ingress -->
- Egress -->


## Creating A VPC

#### 1) Click On services on the top of the AWS dashboard and find VPC

![](/images/Click-on-Services.png)

#### 2) Click on “Your VPC” on the left and then “Create VPC”

![](/images/Your-VPCs.png)


#### 3) Give your VPC an relative name tag and add the IPv4 CIDR block for your VPC

- The /16 means that in order for something to share the same network as our VPC, the first two octets
must match, ```124.11```

- When creating our subnets you will see how we have implemented this

![](/images/Add-VPC-Name.png)

## Creating the Internet Gateway (igw)

#### 1) On the left sidebar click on internet gateway

![](/images/Click-igw.png)

#### 2) Add a conventional name tag and then click on create internet gateway

![](/images/Name-igw.png)

#### 3) We will next click on actions and then attach VPC. We then attach our IGW to
the VPC we have previously made

![](/images/Attach-VPC.png)

## Create a public and private Subnet

As previously stated, our software will be a two-tier architecture and thus we will
have separate subnets for our app and DB

#### 1) Click on subnets located on the left sidebar and then click on create subnet
found on the top

![](/images/Click-on-subnets.png)

#### 2) Using the correct convention, we can now name our subnet, we then enter our VPC id so
our subnet is created within it. We then must give our subnet it's own Ipv4 CIDR block

- Note that the first two octets are the same as our VPC, ensuring it is within it's network.
- We will then change the number in the third octet to differentiate this subnet so that
later we can attack our EC2 to it


![](/images/Name-subnet.png)

#### 3) After clicking create, we will then create another subnet, this time being for our private

- Note how this time the number in the third octet has been changed to 2

## Creating a Route Table

#### 1) On the left sidebar click on route table, then click on the actions and
edit routes

![](/images/Edit-Public-route-table.png)

#### 2) We can then add a destination and target

- 0.0.0.0/0 refers to the whole internet and we attach this to the igw we made earlier, then click create

![](/images/Edit-Routes-Public.png)

#### 3) While still selecting the public route table, we can then click on subnet associations and edit associations located on the bottom of the screen


![](/images/Edit-subnet-associations.png)


#### 4) Now we will associate our public route table with our public subnet

- This is the route instances in our public subnet will take to interact with the
internet

![](/images/public-route-subnet-associations.png)



## Creating A Network ACL

#### 1) On the left sidebar, under security click Network ACL and then create network ACL

![](/images/Create-Network-ACL.png)

#### 2) We will then give it a conventional name and make sure it is attached to the VPC that we had created earlier, then click create

![](/images/Name-Network-ACL-Public.png)

#### 3) Whilst still selecting the public NACL, click on inbound rules and then edit inbound rules

![](/images/Click-Edit-Inbound-Public.png)

#### 4) We will then add the following configurations

- Will explain configuration

![](/images/Edit-Inbound-Rules.png)

#### 5) We then allow all traffic for outbound rules

![](/images/Outbound-allow-all.png)

#### 6) In the subnet associations select the public subnet created earlier and select edit

![](/images/subnet-associations-public-NACL.png)
