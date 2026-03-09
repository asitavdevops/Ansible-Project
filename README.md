# Ansible-Project
#Ansible Concepts : Linux/Ubuntu Mechin
Let for the example  create two servers 1) Ansible Server 2) Target Server 
Note : We will create every thing in Ansible and based on the Ansible Playbook it will perform task in Target Server 
Prerequsits: 1) Install Ansible in Ansible server 2) Create Passowrd less Auth using ssh-keygen 

Step 1  - Connect to Ansible server using PrivateIp (Ubuntu) ec2 instance.(Only PublicIp is working when connecting)
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

Step 5 - Ansible Playbook : When we want to execute mutiple task we use ansible playbook.

        Senario : In all traget servers we want to install Nginx and start Nginx (Write a playbook)
        --> vim first-ansible.yml
        ---(three hyphen stands for yaml file)
        -        name: Install and start Nginx (Name indicates the explanation of the playbook)
                 hosts: all (It will execute all the servers in your inventory files)
                 become: root (It will execute the playbook as a root user , we need to decide same user or root user needed)

                 tasks: (When we want to perform multiple action for each item should start with hyphen)
                  - name: Install Nginx
                    shell: apt install nginx (OR)
                    apt: (Its a package manager module that ansible provides)
                     name: nginx (Here specify which module i want to use)
                     state: present (It means to Install)
                  - name: Start Nginx
                    shell: systemclt start nginx (OR)
                    service: (Its a service module that ansible provides)
                     name: nginx (Here specify which module i want to use)
                     state: started (It means it will start nginx)
                     (save the file)

Note :tasks consider the first task if multiple tasks accordingly we can specify
Run Ansible playbook --> ansible-playbook -i inventory first-ansible.yml
check how ansible executing internally use debug/vervose(v or vv) option --> ansible-playbook -vvv -i inventory first-ansible.yml
To check Nginx installed in target server--> sudo systemctl status nginx

Senario 2: Create 3 Ec2 Instances on AWS (Terraform)
           Configure 1 of those Ec2 Instance as Master(Ansible)
           Configure Other 2 Ec2 Instance as workers (Ansible)
Note: Above Senario will increase complexcity if written in 1 playbook hence to reduce that we use Ansible Role(Multiple playbook).

Ansible Role: An Ansible Role is a way to organize Ansible playbooks into reusable and modular components.
It follows a standard directory structure that separates tasks, variables, files, templates, and handlers.
Roles help make automation clean, reusable, and scalable, especially when managing large infrastructure

Explain why we need Roles :

When we want to configure Kubernates using ansible we need to write 50 to 60 task , lots of variables, parameters,certificates ,
secrets need to configure while creatign this K8 cluster for that very own reason if we try to do it with Roles using ansible-galaxy
we can segrigate each and everything and structure our ansible playbook .

**create/initiate a role for kubernates**
how to define role:  ansible-galaxy role init kubernetes

mkdir secod-playbook ; cd secod-playbook
ansible-galaxy role init kubernates
ls -lrt kubernates/

**Standard Role Directory Structure**
roles/
 └── nginx/
     ├── tasks/
     │    └── main.yml
     ├── handlers/
     │    └── main.yml
     ├── templates/
     ├── files/
     ├── vars/
     │    └── main.yml
     ├── defaults/
     │    └── main.yml
     └── meta/
         └── main.yml

meta/ : Here meta is used to meta data info about the file . We can provide the playbook infor and licencing info . In feature we want to         share the playbook to the Ansible community so it needs the description .
defaults/ : we can store some variables.
vars/ : we can store some variables samething like defaults/
handlers/ : Handlers we use to handle exceptions.
tasks/ : Ansible files performs actions based on tasks
files/ : Basically certificates or any files we want pass. loke we want index.html to pass to task 
templates/ : Use for templating.

#write same Nginx program using ansible role(Ansible-Galaxy)




***Refer for Practice :**
https://github.com/ansible/ansible-examples
                     
                
                         

                         
                
        
        
        
           



         
         
        


