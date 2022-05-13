# Ansible playbooks for Lenovo DM Series (ONTAP)
Lenovo created playbooks that can be used with DM Series (ONTAP) storage systems to help with:
- Provisioning
- Configuring
## Prerequisites
 - Ansible >= 2.9
 - Requests >= 2.20
 - netapp-lib >= 2018.11.13
 - python3 >= 3.6.8
 - ONTAP collection >= 21.16.0
## Installation
1. Install Ansible
```
yum install ansible
```
2. Install the ONTAP Ansible collection from Ansible Galaxy
```
ansible-galaxy collection install netapp.ontap
```
3. Install the Community Crypto Ansible collection from Ansible Galaxy (Required only when using certificate playbook)
```
ansible-galaxy collection install community.crypto
```
4. Install the netapp-lib Python library
```
pip3 install netapp-lib
```
## Usage
Normal:
```
ansible-playbook <playbook>.yml
```
Recommended (for better ouput formatting):
```
ANSIBLE_STDOUT_CALLBACK=yaml ansible-playbook <playbook>.yml
```
To pre-define certain variables, use:
```
ansible-playbook <playbook>.yml --extra-vars "<var>=<value>  <var2>=<value2>"
```
## Playbooks
- Certificate
- Fibre Channel (FCP)
- iSCSI
- NFS
- NVMe
- Security Hardening
- SnapMirror
- SnapShot
- Volume Create
#### Certificate
Playbook to install self signed or signed SSL certificate to a cluster
#### Fibre Channel (FCP)
Playbook to configure and provision an FC target (to be ready for initiator to connect to)<br />
Includes creation of:
- SVM (FC enabled)
- Data LIFs
- LUN
- Initiator Group
#### iSCSI
Playbook to configure and provision an iSCSI target (to be ready for initiator to connect to)<br />
Includes creation of:
- SVM (iSCSI enabled)
- Data LIFs
- LUN
- Initiator Group
#### NFS
Playbook to configure and provision an NFS target (to be ready for initiator to connect to) (Intended for use in clusters with available spare disks to create data aggregates from)<br />
Includes creation of:
- SVM (NFS enabled)
- Data aggregates (local tiers)
- Data LIFs
- Volume
- Export Policy
Intended for use in clusters with available spare disks to create data aggregates from
#### NVMe
Playbook to configure and provision an NVMe target (NVMe-FC/NVMe-TCP) (to be ready for initiator to connect to) (Intended for use in clusters with available spare disks to create data aggregates from)<br />
Includes creation of:
- SVM (NVMe-FC or NVMe-TCP enabled)
- Data aggregates (local tiers)
- Data LIFs (FC or Ethernet)
- NVMe subsystem
- Namespace
#### Security Hardening
Playbook to configure recommended security settings for a cluster, including:
- Login banner
- Minimum number of digits for password: 1
- Minimum number of special characters for password: 1
- Minimum number of lower case letters for password: 1
- Minimum number of upper case letters for password: 1
- Number of unique passwords before allowing re-use: 5
- Number of days for password change delay: 1
- TLS and cipher settings
- Initial password change enforcement (Enabled)<br />
Also to recommend the following:
- Aggregate encryption
- Volume encryption
- vserver auditing
- Using signed certificates
- NTP server configuration
#### SnapMirror
Playbook to configure a SnapMirror relationship between two clusters<br />
Includes creation of:
- SVM peering
- SnapMirror relationship
#### SnapShot
Playbook to create a snapshot for a volume<br />
Includes creation of:
- Snapshot
#### Volume Create
Playbook to provision a LUN, volume or namespace and also to setup to be ready to be consumed by initiator(s) (Intended for use in clusters with SVM, aggregates, etc. already created)<br />
Includes creation of:
- LUN
- Volume
- Namespace
## Hints and Tips
- (security_hardening.yml) The initial password change will be enforced after running the playbook even if the password has already been changed from default. After running the playbook, SSH using the cluster management IP address to be prompted to change the password
- (security_hardening.yml) Volume encryption checks even if you don't have encryption license/encrypted build. In this case, this suggestion can be ignored
- (certificate.yml) Community Crypto Ansible collection is only required for the certificate playbook
## Known Issues
- Upon playbook execution, the following error message is observed:<br />
  "fatal: [localhost]: FAILED! => {"changed": false, "msg": "the python NetApp-Lib module is required"}"<br />
  Fix: Run pip3 install netapp-lib
## Additional Resources
- ONTAP Ansible Module documentation: https://docs.ansible.com/ansible/devel/collections/netapp/ontap/
