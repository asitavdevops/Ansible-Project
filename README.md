# Ansible-Project
#Ansible Concepts : Linux/Ubuntu Mechin
Let for the example  create two servers 1) Ansible Server 2) Target Server 
Note : We will create every thing in Ansible and based on the Ansible Playbook it will perform task in Target Server 
Prerequsits: 1) Install Ansible in Ansible server 2) Create Passowrd less Auth using ssh-keygen 

Step 1  - Connect to Ansible server using PrivateIp (Ubuntu) ec2 instance
        - install Ansible - sudo apt update /sudo apt install ansible Note: Running any apt package manager command we need sudo user
        - check Ansible availablity - ansible --version
Step 2 - Set up Passwordless Authentications (Easiest Way ssh-keygen and other way is ssh-copy-id)
       - When we wanted to configure Target Server(Ansible Playbook) from Ansible server we need to create the passwordless Authentication 
         so that Ansible server can connect to Target serving without using any password so that it can perform any Ansible actions from 
         Ansible server .
         - Connect to Ansible server using Private Ip (Ec2 Instance)
         - ssh-keygen  -> It will generate pubblic key(id_rsa_pub) used to configure, Private key(id_rsa),authorized_keys,known-hosts
         - cat /ubuntu/home/.ssh/id_rsa.pub
         - copy the public key content log in to Target server the do - ssh-keygen 
         - then go to authorized_keys of target server and paste the public key content from Ansible server . Hence now it will authorized
           to connect from Ansible to Target server using public key without prompting for password.

Step 3  - ssh Ip-Address(Target Server)
          Ansible server should be connected to Target Server 
Step 4  - Ansible Adhoc command --> Like Shell commands we can use ansible commands which is called Ansible Adhoc commnds in CLI. 
          Like Shell Scripting we are using Ansible Playbooks to perform Bunch of tasks in Ansible 
          Ansible Playbooks:
        - ansible -i inventory 10.28.23.80 -m "shell" -a "touch Aiops" --> If we have only 1 server
        or ansible -i inventory all -m "shell" -a "touch Aiops" --> all means it will take all the ips from inventory file
         -m --> stands for ansible module
         shell --> Ansible shell module so it will perform shell command
         -a --> stands for Argument
         Note: To check all modules google - Ansible all modules -(ansible.builtin.shell – Execute shell commands on targets)
         (https://docs.ansible.com/projects/ansible/latest/collections/index_module.html)
         Q1) No of processes running in all target servers what is the ansible adhoc command and disk usages .
          - ansible -i inventory all -m "shell" -a "nproc"
          - ansible -i inventory all -m "shell" -a "df"
          Q2) Copy file from Ansible server to Target servers
        - ansible -i ansible all -m "copy" "src: "/srv/myfiles/foo.conf" dest: "/etc/foo.conf" owner: "foo" group: "foo" mode: "'0644'" "
        Q3) Lets say we have DBserver and Webserver in Inventory and we want to execute the adhoc command for differently for DB server
            and webserver. 
            Ans: Server Grouping concept in Inventory files
            inventory:
            [dbservers]
            172.32.58
            172.32.66

            [webservers]
            170.78.0.1
            170.78.01
          -Only Execute the ansible command for Webserves so that it will execute 
          - ansible -i inventory webservers -m "shell" -a "df"

Step 5 - Ansible Playbook :
           



         
         
        


