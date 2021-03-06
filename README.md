# How to Launch a Jupyter Notebook with AWS

**Goal**: Outline all the steps required to configure my own Jupyter Data Science Notebook Server on Amazon Web Services. 

### **1. Creating an ssh Key**  
**a.** Open Git Bash.  
**b.** Create a new ssh key by typing ` ssh-keygen -t rsa`.  
- This will be saved in `~/.ssh/id_rsa`.  
- You can leave the password blank.  

**c.** Check your new key by using `cat ~/.ssh/id_rsa.pub`.          

### **2. Navigating to Amazon EC2**  
**a.** Log into AWS and find the EC2 Dashboard. It should be under the 'compute' tab.  
**b.** Make sure the location is set to Oregon on the upper right corner.  

### **3. Setting Up Key Pairs**  
**a.** On the EC2 Dashboard, select 'Key Pairs'.  
**b.** Select 'Import Key Pair'.  
**c.** Copy and paste the contents from `cat ~/.ssh/id_rsa.pub` into Public key contents.   
**d.** Name your Key pair.  
**e.** Import.  

### **4. Create a New Security Group**  
**a.** On the EC2 Dashboard, select 'Security Groups'.  
**b.** Create Security Group.  
**c.** Name the security group (ex: `juypter_docker`).  
**d.** Write a quick description like "Open access to Jupyter and Docker default ports."  
**e.** Then add the following rules:  
- SSH, Port 22  
- HTTP, Port 80  
- HTTPS, Port 443  
- Custom TCP Rule, Port 2376 (*Docker Hub*)
- Custom TCP Rule, 8888 (*Jupyter*)  

**f.** Change Source to 'Anywhere'.  
**g.** Create.  

### **5. Setting up the AWS Operating System**  
**a.** On the EC2 Dashboard, click the 'Launch Instance' button.  
**b.** First tab is to "Choose an Amazon Machine Image (AMI)," which contains the software needed to run a sandbox machine. Select the latest stable Ubuntu Server that is free-tier eligible.  
**c.** Second tab is to "Choose an Instance Type." Select the t2.micro (eligible for free-tier).  
**d.** Third tab is to "Configure Instance," which can be ignored.  
**e.** Fourth tab is to "Add Storage." Change the size to 30GB.  
**f.** Fifth tab is to "Add Tags." This can be ignored.  
**g.** Sixth tab is to "Configure Security Groups." Select an existing security group and choose the Juypter_Docker group. There will be 8 rules in this group. Be sure to check that port 2376 and 8888.  
**h.** Seventh tab is to "Review Instance Launch." Check that an Ubuntu server is being used, t2.micro is the variable type, and the appropriate rules in the security group is applied. If this is correct, press 'Launch.'  
**i.** Choose an existing key pair and check the box to acknowledge you have read the statement. Then hit 'Launch Instance.' You should be able to see it in the EC2 Dashboard Instances pane.  

### **6. Docker Installation**  
**a.** SSH into EC2 Instance by entering `ssh ubuntu@12.345.678.901 ` on Git Bash. Use the **public** IP address from the instance you just created.  
**b.** Then download docker with `curl -sSL https://get.docker.com/ | sh`.  
**c.** Add the Ubuntu user to the Docker Group with `sudo usermod -aG docker ubuntu`.  
**d.** Restart Docker Daemon with `sudo reboot`. Force quit and reenter the system with `ssh ubuntu@12.345.678.901 `.  
**e.** Check the newly downloaded Docker version by entering `docker -v`.  

### **7. Obtaining the Correct Docker Image**  
**a.** Pull the Docker image you wish to work on with `docker pull jupyter/scipy-notebook`. This takes about a ~1-2 minutes. Once this is complete, it will be present in the docker images cache. Anytime you wish to run a new Jupyter container, Docker will load the container from the image in the cache.  
  
### **8. Running the Correct Docker Image as a Container**  
**a.** Run Jupyter Notebook server in the Docker container with `docker run -p 8888:8888 jupyter/scipy-notebook`.

### **9. Addressing Jupyter Notebook Security Concerns**
**a.** The previous output provided an authenication tocken used to access the Notebook server through a browser. Use the URL provided and replace the `localhost` with the public IP address the EC2 instance is using.

### Overall Diagram
![docker diagram](https://user-images.githubusercontent.com/22715593/34930765-0ad4bd6e-f980-11e7-8eb1-f2a895ba9811.JPG)

### Budget  
The following details the cost of running Jupyter Data Science Notebook Servers using different kinds of EC2 instances for three months: (*3 months will contain about ~90 days. Therefore there will be approximately 2160 hours in three months*)  

| EC2 Instances 	| Price Per Hour 	| Linux Usage Cost 	|  
|---------------	|----------------	|------------------	|  
| t2.micro      	| $.0023         	| $4.97            	|  
| t2.small      	| $.004          	| $8.64            	|  
| t2.medium     	| $.0082         	| $17.71           	|  
| t2.large      	| $.0162         	| $34.99           	|  
| t2.xlarge     	| $.0319         	| $68.90           	|  
| t2.2xlarge    	| $.0653         	| $140.83          	|  
