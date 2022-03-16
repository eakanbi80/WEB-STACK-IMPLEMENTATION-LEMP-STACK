# WEB STACK IMPLEMENTATION LEMP STACK

Welcome back! In project one, we focused on deploying the LAMP stack, in this project we'll hone in on LEMP. The term LEMP is an acronym that represents:

**L** –– Linux Operating System >> An operating system that manages all of the hardware resources associated with your desktop or laptop, a.k.a., the communication between your software and hardware

**E** –– Nginx Server **(pronounced as engine-x)** >> A high-preforming web server that enables the processing of many requests at the same time

**M** –– MySQL Database >> An open-source relational database management system

**P** –– PHP (Hypertext Preprocessor)

The Nginix server used in this project, is the main difference in LEMP versus LAMP.

## LINUX

### Setting up your virtual environment

We'll need an AWS account and a virtual server with Ubuntu Server OS. If you don't have one already, follow the steps in my [LAMP Stack](https://github.com/eakanbi80/web-stack-implementation-lamp) project to create a free tier AWS account.

![](./image2/image1new.png)

Once you have successfully signed-in to your AWS account, navigate to the top-right of the screen and select your preferred region––this should be the closest region to your physical location.

![](./image2/image2new.png)

You'll then want to navigate to the search bar and type in 'EC2.' Select the EC2 service that appears on top.

![](./image2/image3.png)

Click on the 'Launch Instances' button that appears in the top right side of your screen.

![](./image2/image4.png)

Proceed by selecting the Ubuntu Server 20.04 LTS (HVM) option as the Amazon Machine Image (AMI).

![](./image2/image5.png)


On the following page, locate and select t2.micro as the instance type and click 'Review and Launch.'

![](./image2/image6.png)

Next, click on 'Launch' at the bottom of the screen.

![](./image2/image7.png)

A window should appear asking you to create a key pair. Make sure you create one and then select 'Download Key Pair.' **It's important to know the location the file was downloaded to not lose the .pem file. You will need this file in order to connect into your server from your local PC. After you've downloaded the key pair, check the box for the acknowledgement, and then click on "Launch Instances".**

![](./image2/image8.png)

Awesome, you've successfully launched an EC2 instance!

![](./image2/image9.png)

To view your new instance, click the 'View Instances' button at the bottom-right of the screen.

![](./image2/image10.png)

# Connecting to your EC2 from your local PC

**FRIENDLY REMINDER**–– Anchor tags(< >) will be used to indicate contents that must be replaced with your unique values. For example, if you have a file named "keypair123.pem" you must enter this information within the corresponding anchor tag: < private-key-name >

Let's connect to our instance!

Begin by opening Terminal. Once you have opened Terminal, use the 'cd' command to change into the directory that your key pair is located. This is usually the ~/Downloads directory. If you are having difficulty finding it, you can use the 'ls' command to list the contents of your current directory.

Once you have located the key pair, use the command below to activate the key file (.pem). This command will also change permissions (otherwise you may get the error “Bad Permissions”):
```
$ sudo chmod 0400 .pem When prompted, type the password for your local PC and press Enter on your keyboard.
```

When prompted, type the password for your local PC and press Enter on your keyboard.

Next, go back to the AWS console for a moment, and navigate to your running EC2 instance. Copy the Public IP address, as shown in the image below:

![](./image2/image11.png)

Once you have copied the Public IP address, head back to your terminal. You'll want to connect to the EC2 instance by using the following command:

```
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
```

You will be asked if you want to continue connecting. Type 'Yes' and press 'Enter' on your keyboard.

![](./image2/image12.png)

When connected, your ip-address will be shown on your terminal.

![](./image2/image13.png)

# Installing the Nginx Web Server

Employing Nginx, a high-performance web server, will help us to display web pages to site visitors. First, we'll need to use the apt package manager to install this package. Run the following commands below to get Nginx installed:

```
$ sudo apt update
```

![](./image2/image14.png)

Install Nginx by following the command below:

```
$ sudo apt install nginx
```
When prompted, enter 'Y' to confirm installation.

![](./image2/image15.png)

To verify that your installation was successful and Nginx is now running on your Ubuntu server run the follwing command on your terminal:

```
$ sudo systemctl status nginx
```

If there is a green dot, then you've completed all steps successfully, and it's running.

![](./image2/image16.png)

Prior to receiving any traffic by our Web Server, we need to open TCP port 80––This is the default port that web browsers utilize in order to access web pages on the Internet.

The EC2 instance on the AWS console we created earlier, opened the TCP port 22 by default. This allowed us to access the EC2 via SSH in Terminal. Now, we must add a rule to the security groups of our EC2 configuration, in order to allow inbound connections through port 80.

Open the AWS Management Console. Click on the security group tab and edit the inbound rules of the running EC2 instance.

![](./image2/image17.png)

Click on Edit Inbound Rules

![](./image2/image18.png)

Click on 'Add Rule' and add the HTTP, TCP port 80 and allow source from anywhere by using 0.0.0.0, allowing traffic from any IP address to enter.

![](./image2/image19.png)

Our server is now running and we can access it locally and from the Internet. We'll now test to see whether or not we can receive traffic. Run the following command to request our Nginx on port 80:
```
$ curl http://localhost:80
```
![](./image2/image20.png)

Now we'll test and verify our Nginx server access, using the public IP address of the EC2 instance via web browser.

```
http://<Public-IP-Address>:80
```

The following screenshot is an example of how the Nginx web default page should look like, if no hiccups have been made.

![](./image2/image21.png)

# Installing MySQL

Now that our web server Nginx is up and running, we'll focus on installing MySQL. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project. First, we're going to install a Database Management System (DBMS) to be able to store and manage data for our site in a relational database.

plug in the command to install MySQL:

```
$ sudo apt install mysql-server
```

When asked, confirm that you want to proceed with the installation by typing Y for "Yes", and then press 'Enter' on your keyboard.

![](./image2/image22.png)

Once the installation is complete, run a security script in order to add more security access to the database system. The following command will help you do so:

```
$ sudo mysql_secure_installation
```

When asked to validate password component. Type 'Y' for Yes.

After, choose the level of your password validation. There are three levels of password validation policy:

Choose either 0 = LOW, 1 = MEDIUM or 2 = STRONG

![](./image2/image23.png)

Once you've set a password for the MySQL root user, type 'Y' for Yes and press 'Enter' on your keyboard at each prompt.

These security measures will remove anonymous users and the test database, disable remote root logins, and then reload these new rules so that the changes will be reflected on the MySQL database.

![](./image2/image24.png)

Next, test if you’re able to log in to the MySQL console by typing the following command:

```
$ sudo mysql
```

The command above will connect to the MySQL server as the administrative database user root. You should expect to see an output like this:

![](./image2/image25.png)

To exit the MySQL console, follow the command:

$ mysql> exit

# Installing PHP




