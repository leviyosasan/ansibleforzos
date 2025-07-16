# Ansible for z/OS

Bu doküman, Ansible kullanarak z/OS sistemlerine nasıl bağlanılacağını ve bir JCL işini nasıl çalıştıracağınızı adım adım anlatır.

---

## 1. Gereksinimler

- Ubuntu Server
- Ansible
- sshpass
- IBM z/OS erişimi (SSH ile)

---

## 2. Ubuntu Server Kurulumu

Ubuntu Server'ı aşağıdaki bağlantıdan indirip kurun:

🔗 [Ubuntu Server İndir](https://ubuntu.com/download/server)

---

## 3. Ansible Kurulumu

Ubuntu kurulumunu tamamladıktan sonra, Ansible kurulumuna geçin:

```bash
sudo su
apt update
apt install ansible -y
ansible-galaxy collection install ibm.ibm_zos_core
 ```````

4. sshpass Kurulumu
Ansible, z/OS'e bağlanmak için SSH kullanır. Bu nedenle sshpass aracını kurmanız gerekir:
```bash
apt update
apt install sshpass -y
 ```````
5. Envanter (inventory) Dosyası Oluşturma
Bağlantı yapılandırması için bir inventory.yaml dosyası oluşturun:
```bash
nano inventory.yaml
 ```````
Aşağıdaki içeriği ekleyin:
```bash
all:
  hosts:
    zos_host:
      ansible_host: host_address
      ansible_user: tso_user
      ansible_ssh_pass: "tso_user_password"
      ansible_python_interpreter: "/python_path"
      cmd_dir: "/python_path/bin"
 ```````

6. Ansible Playbook Oluşturma
Bir JCL işini çalıştırmak için bir playbook.yaml dosyası oluşturun:
```bash
nano playbook.yaml
 ```````
Aşağıdaki içeriği ekleyin:
```bash
---
- name: Submit a JCL without mvscmdauth
  hosts: zos_host
  gather_facts: no
  tasks:
    - name: Submit JCL using shell
      raw: "submit '//DATASET(MEMBER)'"
 ```````
7. Playbook’u Çalıştırma
Hazırladığınız playbook'u çalıştırmak için aşağıdaki komutu kullanın:
```bash
ansible-playbook -i inventory.yaml playbook.yaml
 ```````

