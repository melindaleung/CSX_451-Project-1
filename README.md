# CSX_451-Project-1

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
**g.** Sixth tab is to "Configure Security Groups."

