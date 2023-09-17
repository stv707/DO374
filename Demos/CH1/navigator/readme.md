
### Install ansible-navigator 

`` 
sudo yum install ansible-navigator 
``

### Login to registry hub and set Image Variable and run the playbook 

``
podman login hub.lab.example.com
export EE=hub.lab.example.com/ee-supported-rhel8:latest
ansible-navigator run playbook.yml --eei $EE
ansible-navigator run playbook.yml --eei $EE -m stdout
``