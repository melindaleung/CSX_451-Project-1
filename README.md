# CSX_451-Project-1

**Goal**: Outline all the steps required to configure my own Jupyter Data Science Notebook Server on Amazon Web Services. 

**1. Creating an ssh Key**  
  a. Open Git Bash.  
  b. Create a new ssh key by typing ` ssh-keygen -t rsa`.  
    - This will be saved in `~/.ssh/id_rsa`.  
    - You can leave the password blank.  
  c. Check your new key by using `cat ~/.ssh/id_rsa.pub`.          

**2. Navigating to Amazon EC2**  
a. Log into AWS and find the EC2 Dashboard. It should be under the 'compute' tab.  
b. Make sure the location is set to Oregon on the upper right corner.  

**3. Setting Up Key Pairs**  
a. On the EC2 Dashboard, select 'Key Pairs'.  
b. Select 'Import Key Pair'.
c. Copy and paste the contents from `cat ~/.ssh/id_rsa.pub` into Public key contents.   
d. Name your Key pair.  
e. Import.  

**4. Create a New Security Group**  
a. 
