# ANSIBLE PROJECT 11

# *STEP 1:Install and Configure Ansible on ec-2 Instance*

1. Our ec2 instance name tag was up dated to *JENKINS-ANSIBLE*
![Alt text](<Ansible project/step 1/jenkins-ansible server.jpeg>)

2. A Github repository was created and named *ansible-config-mgt*
![Alt text](<Ansible project/step 1/git repo.jpeg>)

3. Ansible was installed with the below codes;
    `sudo apt update`
    `sudo apt install ansible`

4. Next, we confirmed the Ansible version by running the code;
    `ansible --version`
![Alt text](<Ansible project/step 1/ansible download.jpeg>)  

5. Jenkins build job was configured to archive our repository content everytime a change is made
    * A new freestyle project ansible was created in jenkins and pointed to my *ansible-config-mgt* repository in github
    ![Alt text](<Ansible project/step 1/ansible freestyle.jpeg>)
    * Github webhook was set to trigger project ansible in jenkins
    ![Alt text](<Ansible project/step 1/webhook config.jpeg>)
    * Post build was configured  to save all (**) files
    ![Alt text](<Ansible project/step 1/jenkins post build.jpeg>)

6. Next is to test that changed in the README.md file build automatically and jenkins saves the file in the below folder;
    `/var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/`


# *STEP 2:Development Environment Preparation using Visual Studio Code*

1. Download a Visual Studio Code

2. After installation of the VS Code, it was configured to connect to my git repository

3. I then cloned my *ansible-config-mgt* github repositiory via VS Code

4. Remote development pack was downloaded in my VS code


# *STEP 3:Begin Ansible Development*

1. A new branch was created that was used for development of a new feature
![Alt text](<Ansible project/step 3/new_branch_created.png>)

2. I checked out to the newly created branch on my local machine and started building

3. Playboom directory was created to store all playbook files
![Alt text](<Ansible project/step 3/playbooks folder created.png>)

4. A new directory named inventory was created
 ![Alt text](<Ansible project/step 3/inventory directory created.png>)

5. Within the playbook file, my first playbook named common.yml was created

6. Within the inventory directory, development, staging, testing and production files were created
![Alt text](<Ansible project/step 3/files created.png>)


# *STEP 4:Set up an Ansible Inventory*

An Ansible inventory file defines the host and group of hosts which commands modules and tasks in a playbook operates.

1. I added ssh agent using the below code;
eval `ssh-agent -s`
![Alt text](<Ansible project/Step 4/ssh key added.png>)

2. Next was to add the key. This was done with the below code;
`ssh-add <path-to-private-key>`
![Alt text](<Ansible project/Step 4/ssh key added.png>)

3. Inventory/dev.yml file was updated with the private internet protocol of all servers we want to work on
![Alt text](<Ansible project/Step 4/inventory_dev.yml update.png>)


# *STEP 5:Create a Common Playbook*

Command was given to Ansible on what i need to be performed on all servers listed in the Inventory/dev.yml

Below was the command given to Ansible;

`---`
`- name: update web, nfs and db servers`
  `hosts: webservers, nfs, db`
  `become: yes`
  `tasks:`
    `- name: ensure wireshark is at the latest version`
      `yum:`
        `name: wireshark`
        `state: latest`
   

- `name: update LB server`
  `hosts: lb`
  `become: yes`
  `tasks:`
    `- name: Update apt repo`
      `apt:` 
        `update_cache: yes`

    `- name: ensure wireshark is at the latest version`
      `apt:`
        `name: wireshark`
        `state: latest`
![Alt text](<Ansible project/Step 5/playbook_config.png>)


# *STEP 6:Git Update with Latest Code*

All directories and files live on the local machine and needed to be pushed to the the github repository so it can communicate to our jenkins and jenkins in turn communicate to our aws Jenkins-Ansible server

1. The below git command was used to achieve this;
`git init`
`git add .`
`git commit -m "Ansible"`
`git branch -M "main"`
`git remote add origin "repo link"`
`git push -u origin main`

2. Pull request was created then i acted as the second developer to review and merged.


# *STEP 7:Run the First Ansible Test*

1. I cd into the directory where i have the Inventory and Playbook on my Ansible-Jenkins Server

2. I ran the playbook with the below code;
`ansible-playbook -i inventory/dev.yml playbooks/common.yml`
![Alt text](<Ansible project/step 7/playbook1.png>)
![Alt text](<Ansible project/step 7/playbook2.png>)

3. I ssh into the servers to verify that our playbook ran correctly
![Alt text](<Ansible project/step 7/wireshark confirmation.png>)

















