# Instruction - Ansible Practice

### Pre-requisites
  
1. <b>Spin AWS Instances:</b></br>
    Follow the below repository for deploying the instances which will be configured in Kubernetes Cluster Setup.</br>
    `No of Instances: 2`

      <a href="https://github.com/avasant21/terraform-practice" target="_blank" rel="noopener noreferrer">https://github.com/avasant21/terraform-practice</a></br>

2. <b>OS Configurations:</b></br>
    Follow the below steps to configure required OS Hostname to be used in Ansible Inventory. Use the Terraform output private IPs to replace the IP Entries and update `/etc/hosts` file</br>

        $ cat <<EOF | sudo tee /etc/hosts
        <IP Instance 1>      master
        <IP Instance 2>      web01
        EOF

3. <b>Copy Private Key into Master:</b></br>
    Follow the below instructions to copy the private key to master which can be used for Ansible master & target communication. Copy the content of `test-key.pem`(key created for Terraform deployment) in below file.</br>

        $ cat <<EOF | sudo tee /etc/ansible_centos.pem
        <Content of test-key.pem>
        EOF
        
        $ sudo chmod 600 /etc/ansible_centos.pem

### Ansible Installation & Configuration - Based on Centos 7

1. Package Repo Configuration</br>
    Ansible package is not available in the default yum repositories, so we will enable epel repository for CentOS 7 using below commands.</br>

        $ sudo yum install epel-release -y

2. Ansible Installation</br>
    Install Ansible using yum command.</br>

        $ sudo yum install ansible -y

3. Ansible Version</br>
    Once the installation is completed, check the ansible version.</br>

        $ ansible --version
        
4. Inventory Configuration</br>
    The inventory can be configured as below to enable ansible to know hosts.</br>

        $ cat <<EOF | sudo tee -a /etc/ansible/hosts
        [webserver]
        web01
        
        [webserver:vars]
        ansible_user=centos
        ansible_ssh_private_key_file=/etc/ansible_centos.pem
        EOF
        
