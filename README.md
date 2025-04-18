# README for Ansible Playbooks

This README provides an overview and explanation of the three Ansible playbooks included in the repository. These playbooks are used to provision EC2 instances, install Nginx on remote servers, and upload SSH keys to target hosts.

## Table of Contents:
1. **Provision EC2 Instances with Ansible**
2. **Install Nginx on Remote Hosts**
3. **Upload Public SSH Key to Target Hosts**

---

## 1. Provision EC2 Instances with Ansible

**File Name:** `provision_ec2.yml`

### Purpose:
This playbook is designed to provision an EC2 instance in AWS using Ansible. It creates a security group, launches an EC2 instance, and outputs the instance details such as its public IP address.

### Key Sections:

- **Hosts**: `localhost` (The playbook runs locally, typically on the Ansible controller machine).
  
- **Variables**:
  - `keypair`: The name of the AWS EC2 key pair.
  - `instance_type`: The type of the EC2 instance (e.g., `t2.small`).
  - `image`: The AMI ID for the instance.
  - `region`: The AWS region where the EC2 instance will be launched (e.g., `us-east-1`).
  - `security_group`: The name of the security group to be created for the instance.
  - `count`: The number of EC2 instances to launch (default: 1).

- **Tasks**:
  1. **Create Security Group**: Defines a security group with rules allowing inbound traffic on ports 22 (SSH), 80 (HTTP), and 8080 (custom).
  2. **Launch EC2 Instance**: Uses the `ec2_instance` module to launch an EC2 instance with the specified parameters.
  3. **Debug EC2 Instance Output**: Prints the output from the EC2 instance creation.
  4. **Display Instance Information**: Outputs the instance ID and public IP address of the launched EC2 instance(s).

---

## 2. Install Nginx on Remote Hosts

**File Name:** `install_nginx.yml`

### Purpose:
This playbook installs Nginx on remote hosts and ensures the Nginx service is running and enabled on startup.

### Key Sections:

- **Hosts**: `remote_hosts` (The target hosts where Nginx will be installed).
  
- **Tasks**:
  1. **Update apt Repository**: Updates the apt package cache to ensure the latest package versions are available.
  2. **Install Nginx**: Installs the `nginx` package if it's not already installed.
  3. **Ensure Nginx is Running and Enabled**: Ensures the Nginx service is started and will be enabled on boot.

---

## 3. Upload Public SSH Key to Target Hosts

**File Name:** `upload_ssh_key.yml`

### Purpose:
This playbook uploads a public SSH key to target hosts, enabling SSH access using that key.

### Key Sections:

- **Hosts**: `all` (This playbook applies to all hosts defined in your inventory file).
  
- **Variables**:
  - `ssh_public_key`: The public SSH key that will be uploaded. By default, this uses the `id_rsa.pub` key located in the home directory of the user running the playbook.

- **Tasks**:
  1. **Ensure `.ssh` Directory Exists**: Checks if the `.ssh` directory exists in the home directory of the `ubuntu` user and creates it if necessary.
  2. **Upload SSH Public Key**: Uses the `authorized_key` module to upload the public SSH key and add it to the `authorized_keys` file of the target host.
  3. **Set Correct Permissions on `authorized_keys`**: Ensures the correct file permissions (`0600`) are set on the `authorized_keys` file for security.

---

### Prerequisites:
Before running these playbooks, ensure the following:

- Ansible is installed on the local machine.
- AWS CLI and credentials are configured if you're using the EC2 provisioning playbook.
- The remote hosts are accessible via SSH for the Nginx installation and SSH key upload tasks.
- The correct SSH key (`~/.ssh/id_rsa.pub`) is available for uploading to remote hosts.

---

### Running the Playbooks:

To run any of the playbooks, use the following command:

```bash
ansible-playbook <playbook_file.yml> -i <inventory_file>
