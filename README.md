# Comprehensive Documentation for Deploying a Simple Web Server on AWS

## **Introduction**
This documentation provides a detailed walkthrough on how to provision a server, set up a web server, deploy a static HTML landing page, and configure networking using Amazon Web Services (AWS). The goal is to create a robust environment while making thoughtful choices between multiple options at each step. This guide is tailored for professional standards and is suitable for GitHub documentation.

---

## **Project Overview**
We aim to:
1. Provision a virtual server (EC2 instance) on AWS.
2. Install and configure the apache2 web server.
3. Deploy a simple HTML landing page.
4. Configure networking for public accessibility.
5. (Optional) Secure the web server with HTTPS using Let’s Encrypt.

---

## **Step 1: Provisioning the Server**

### **1.1. Log in to AWS Console**
1. Navigate to [AWS Management Console](https://aws.amazon.com/console/).
2. Create an AWS account if you don’t have one.

> **Why AWS EC2?**
AWS EC2 is widely used for scalable computing. It offers flexibility, cost-efficiency, and global infrastructure. Compared to other providers (e.g., Google Cloud or Azure), AWS provides excellent free-tier options for new users.

### **1.2. Launch EC2 Instance**
1. In the AWS Console, search for **EC2** under the Compute section.
2. Click **Launch Instance** to create a new virtual server.

### **1.3. Choose an Amazon Machine Image (AMI)**
- **Option Chosen:** Ubuntu Server 22.04 LTS (Long Term Support).

### **1.4. Select an Instance Type**
- **Option Chosen:** `t2.micro` (eligible for AWS Free Tier).

### **1.5. Configure Instance Details**
1. Enable **Auto-assign Public IP** to ensure the server is accessible over the internet.
2. Leave other settings as default unless specific requirements dictate otherwise.

### **1.6. Add Storage**
- **Option Chosen:** Default 8 GB General Purpose SSD.
- **Reasoning:** Sufficient for hosting a static landing page.

### **1.7. Configure Security Group**
- Add the following rules:
  - **SSH (Port 22):** Allow from your IP address for secure server access.
  - **HTTP (Port 80):** Allow from anywhere (0.0.0.0/0) to enable web traffic.
  - **HTTPS (443):** Use Let’s Encrypt or a hosting provider’s SSL feature to enable HTTPS for your site 

> **Security Consideration:** Limiting SSH access to your IP mitigates brute-force attacks.

### **1.8. Key Pair**
1. Create a new key pair or use an existing one.
2. Save the (Webserver.pem) file securely as it’s required for SSH access.

### **1.9. Launch the Instance**
Click **Launch Instance** and wait for provisioning to complete.

### **1.10. Access the Server**
1. Copy the public IP address(3.9.171.45) of your instance from the AWS Console.
2. Navigate to the directory the keypair is stored on your local machine
3. Connect via SSH:
   
   ssh -i "Webserver.pem" ubuntu@ec2-3-9-171-45.eu-west-2.compute.amazonaws.com

## **Step 2: Installing the Web Server**

### **2.1. Update the Server**
Run the following commands to ensure the latest security patches and updates are installed:

sudo apt update && sudo apt upgrade -y


### **2.2. Install apache2**
1. Install the apache2 web server:
   
   sudo apt install apache2
   
2. Start the service:
   
   sudo systemctl start apache2
   
3. Enable it to start on boot:
   
   sudo systemctl enable apache2
   

> 

### **2.3. Verify apache2 Installation**
1. Open a browser and visit your public IP 3.9.171.45
2. You should see the default apache2 welcome page.

---

## **Step 3: Deploying the HTML Page**

### **3.1. Create the Directory**
Create a directory to host your HTML page:

sudo mkdir -p /var/www/html/Website


### **3.2. Create the HTML File**
1. Navigate to the directory:
   
   cd /var/www/html/Website
   
2. Use a text editor to create an HTML file:
   
   sudo nano index.html
   
3. Copy website files(html,css and other resources) from local storage to server>
   ```

### **3.3. Configure apache2**
Update the default apache2 configuration:
1. Open the configuration file:
   
   sudo nano /etc/apache2/sites-available/default
   
2. Modify the root directive:
   
   root /var/www/html/Website;
  
3. Test the configuration:
   bash
   sudo apache2 -t
   
4. Reload apache2:
   
   sudo systemctl reload apache2
   

### **3.4. Verify Deployment**
Visit 3.9.171.45 in a browser to see your custom landing page.



## **Step 4: Networking Configuration**

### **4.1. Security Group Review**
Ensure the security group allows HTTP (port 80),HTTPS (port 443)and SSH (port 22).

### **4.2.  DNS Configuration**
To associate a domain name with your EC2 instance, configure DNS in your domain registrar and point it to the public IP address of your instance.
 1. Obtain domain name (techbyclancy.com) from Hoststinger.com

 2. On your Aws-console navigate to Amazon Route53

 3. Create an Hosted-Zone with the custom domain name created on hoststinger.com.

 4. Configure the Hosted-zone by defining records and attacthing the public ip-address to the record of the hosted-zone.

 5. Copy the values/route traffic from the Type NS of the hosted-zone (techbyclancy.com) records

 6. Head back to hostinger.com and configure the name server of your domain by changing the values  to the one copied  from the hosted-zone record. 

## **Step 5: Securing the Server with HTTPS**

### **5.1. Install and Configure Certbot**
Install Certbot for obtaining SSL certificates:

sudo apt install certbot3 python3-certbot-apache2 

sudo certbot --apach2 -d techbyclancy.com -d www.techbyclancy.com

### **5.2. Obtain an SSL Certificate**
Run Certbot:

sudo certbot --apache2

Follow the prompts to configure HTTPS automatically.

### **5.3. Verify HTTPS**
Visit https://www.techbyclancy.com/ to confirm SSL is enabled.

---

## **Conclusion**
This documentation has outlined a professional approach to provisioning an AWS EC2 instance, installing and configuring apache2, deploying a static HTML page, and securing the server with HTTPS. The choices made at each step prioritize simplicity, performance, and security. By following these steps, you’ve built a scalable and secure web server hosted on AWS.
