Ansible For z/OS

Ansible for zOS Installation
To install Ansible for z/OS, you first need an Ubuntu server
 ➢ Download: https://ubuntu.com/download/server

After completing the Ubuntu installation, proceed with the Ansible installation.
 ➢ sudo su
 ➢ apt update
 ➢ apt install ansible -y
 ➢ ansible-galaxy collection install ibm.ibm_zos_core

Since Ansible relies on SSH connections, it is necessary to install sshpass.
 ➢ apt update
 ➢ apt install sshpass -y

To establish a connection to z/OS, it is necessary to create an inventory.yaml file.
➢ nano inventory.yaml
all:
  hosts:
    zos_host:
      ansible_host: host_address
      ansible_user: tso_ıser
      ansible_ssh_pass: "tso_user_password"
      ansible_python_interpreter: "/python_path"
      cmd_dir: "/python_path/bin"
      
Let's now write a playbook to execute a JOB on z/OS. Below is an example of how you can structure 
the playbook:

➢ nano playbook.yaml

---
- name: Submit a JCL without mvscmdauth
  hosts: zos_host
  gather_facts: no
  tasks:
    - name: Submit JCL using shell
      raw: "submit '//DATASET(MEMBER)'"
  
We can run the playbook with the following command:
➢ ansible-playbook -i inventory.yaml playbook.yaml
