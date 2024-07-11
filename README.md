# **Sales Dashboard deployed in Azure**
This report details my journey of learning Django, from the initial setup and installation to advanced topics such as integrating with Azure services, implementing load balancers, and setting up virtual machine scale sets. The primary goal of this project was to develop a comprehensive understanding of Django for building robust web applications and leveraging cloud infrastructure for scalability and high availability.

# Setup and Installation
## Prerequisites
1.Python: Ensure Python is installed on your system. Django is a Python-based web framework.<br>
<code>python== 3.5 or up and django==2.0 or up</code><br>

2.Virtual Environment: Create a virtual environment to manage dependencies separately for the project.

3.Django: Install Django within the virtual environment using 'pip install django'.

## Steps - 
1. Create a Virtual Environment:<br>
<code>python -m venv myenv
source myenv/bin/activate  # On Windows use `myenv\Scripts\activate`</code><br><br>
2. Install Django:<br>
<code>pip install django</code><br>
3. Create a Django Project:<br>
<code>django-admin startproject myproject
cd myproject</code><br>

# Installing -<br>
1. Open terminal and clone the git -<br>
<code>git clone https://github.com/athvrvv/sales-dashboard.git</code><br>
or simply download using the url below<br>
<code>https://github.com/athvrvv/sales-dashboard.git</code><br>
2. To migrate the database open terminal in project directory and type<br>
<code>python manage.py makemigrations
python manage.py migrate</code><br>
3. Create superuser for admin panel <br>
<code>python manage.py createsuperuser</code><br>
4. Finally run program in local server using <br>
<code>python manage.py runserver</code><br>
Then go to http://127.0.0.1:8000 in your browser<br>

# Tutorial Progress-<br>
## Basic Concepts Covered -<br>
1. Django Project Structure: Understanding the files and directories created by Django.
2. Creating a Django App: Adding an app to the project to handle specific functionality.<br>
<code>python manage.py startapp myapp</code><br>
3. Models and Migrations: Defining models and running migrations to create database schema.<br>
4. Admin Interface: Configuring the Django admin interface for managing data.<br>
# Key Features Implemented - <br>
1. URL Routing: Configuring URLs to route requests to appropriate views.
2. Views and Templates: Creating views to handle requests and templates to render HTML.
3. Forms and Validation: Building forms for user input and handling validation.
4. User Authentication: Implementing user registration, login, and logout functionality.<br>

# Project Snapshots - 

## Login Page - 

![E6D5B0B8-DFBF-4739-94B5-5A2B009F361C](https://github.com/athvrvv/sales-dashboard/assets/144785958/afcf3b9e-5064-4601-8717-a8d4cada53bd)
<br>
## Profile Page - 
![5DD89816-31DD-485A-BF93-5AAB8A5B395A](https://github.com/athvrvv/sales-dashboard/assets/144785958/68efdd3f-f467-4c61-955f-b36345238baa)
<br>
## Transaction Page - 
![FF8129F2-8BF7-4A0A-9237-2D5EC6F22EA3](https://github.com/athvrvv/sales-dashboard/assets/144785958/885260d2-af44-44ea-b04c-984a259dfda4)
<br>
## Analytics Page - 
![C3989169-3D30-4057-BC46-5EF7C49D77A0](https://github.com/athvrvv/sales-dashboard/assets/144785958/39f43930-2314-4697-a873-fbb5caacac3b)
<br>
## Challenges and Solutions - 
1. Database Migrations: Faced issues with database migrations initially, resolved by understanding the migration process and using Django's built-in tools effectively.<br>
2. Form Validation: Implemented custom validation logic to ensure data integrity and provide feedback to users.<br>
# Azure Integration - <br>
## Setting Up Azure Virtual Machines-<br>
1. Provisioning VMs: Created virtual machines on Azure for deploying the Django application.<br>
2. Configuration: Configured the VMs with necessary software and security settings.<br>
SSH is mainly preferred for linux based virtual machines as the linux based vm runs on the command prompt itself .

<img width="482" alt="Screenshot 2024-07-11 at 11 05 16 PM" src="https://github.com/athvrvv/sales-dashboard/assets/144785958/676eef33-a5f9-4438-b6e4-6f3a5f1164bd"><br>
## Load Balancer Implementation-
1. Creating Load Balancer: Set up an Azure Load Balancer to distribute traffic across multiple instances of the web application.<br>
2. Backend Pools and Health Probes: Configured backend pools and health probes for load balancing and monitoring instance health.<br>
3. A minimum of two linux virtual machines are to be used to display a webapp .Both of them should be in the backend pool of a load balancer .<br>
4. The load balancer should be configured with frontend public ip configuration, load balancing
rules, NAT rules, WAF policy ,etc.<br>
5. If traffic is high on one vm , the load balancer should successfully divert the traffic from that vm and display the webpage in the second vm, balancing the traffic between the two vms.<br>

## Virtual Machine Scale Sets-
1. Auto-scaling: Implemented virtual machine scale sets to automatically scale the number of instances based on load.<br>
2. Integration with Load Balancer: Ensured seamless integration of VM scale sets with the load balancer for efficient traffic management.<br>

## Working -
Scaling is the ability to scale our virtual machines according to a set criteria i.e. we can increase or decrease the number of virtual machines according to our requirement . in this also there are two options provided- manual scaling and auto scaling or custom scaling . in this project I was tasked with autoscaling . in auto scaling we can provide our own criteria for the scaling of virtual machines in the scale set . for example, criteria can be cpu usage or the time the cpu is running,and scaling can be done. Also , a virtual network is selected along with a public ip address. There is also the option to include load balancer but in this case I did not include the load balancer and proceeded with the virtual machine scale set only.
After configuring the virtual machine scale set and setting initial count of virtual machines as 1 I deployed the resource. After successful deployment two instances(virtual machines) were formed in the virtual machine scale set. These virtual machines were running on linux server. So I launched the vms using ssh and activated the linux environment.
After that I installed a package called stress to load test the virtual machines using the command – sudo apt-get install stress -y
The package got installed , now to increase the load, I used the command – sudo stress –cpu 90 to increase the load to 90%, on checking the monitoring tab, the load was increased and after a few minutes a new virtual machine was incremented and total virtual machine count upgraded from 1 to 2 as the criteria for incrementing was cpu load > 75%.
Similarly I tried testing it with decreasing the load , by using the command – sudo stress –cpu 20 to decrease the load to 20%,on checking the monitoring tab, the load was decreased and after a few minutes the new virtual machine was removed and total virtual machine count upgraded from 2 to 1 as the criteria for decrementing was cpu load < 25%.
This was the successful testing of the virtual machine scale set.

## Stress package in Terminal - 

<img width="429" alt="Screenshot 2024-07-11 at 11 13 41 PM" src="https://github.com/athvrvv/sales-dashboard/assets/144785958/1d102661-d329-4fb2-9960-19b14fb0cb66">
<br>

## Virtual Machine Scale Set Autoscaling - 

<img width="482" alt="Screenshot 2024-07-11 at 11 15 43 PM" src="https://github.com/athvrvv/sales-dashboard/assets/144785958/8ee8db37-2305-46dc-9df5-3de6d553f532">
<br>
After setting conditions for autoscaling we increase the stress in using stress package in terminal to test both the load balancer and virtual machine scale set, after increasing stress-
<img width="482" alt="Screenshot 2024-07-11 at 11 17 51 PM" src="https://github.com/athvrvv/sales-dashboard/assets/144785958/80b18eb2-71b5-48cc-9e8c-26f2cb113673"><br>
we can see the notification sent by azure to our gmail that capacity has changed from 1 to 2 virtual machines, that means both the load balancer and vmss are working properly.<br>

## Load Balancer -
<img width="482" alt="Screenshot 2024-07-11 at 11 20 50 PM" src="https://github.com/athvrvv/sales-dashboard/assets/144785958/bb8e5d1d-c6ee-4115-9eb0-9da9d97120d6"><br>
we can also confirm the working by checking the vmss tab in azure - 
<img width="482" alt="Screenshot 2024-07-11 at 11 21 42 PM" src="https://github.com/athvrvv/sales-dashboard/assets/144785958/bd34ca38-96c0-4bc6-b84e-1be2f6eadc93"><br>

# Conclusion-
This project provided a comprehensive learning experience in building and deploying Django applications with cloud infrastructure. By integrating advanced topics such as load balancing and auto-scaling on Azure, I gained valuable insights into creating scalable and high-availability web applications. The knowledge and skills acquired during this project will be instrumental in future endeavors involving web development and cloud technologies.







