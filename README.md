## Weatherapp

Application is deployed in AWS and available under URL **http://weathercast.dwolnicki.net:8000/**


### Prerequisites

Application is based on docker containers.
The runtime environment preparation and application management (deployment/undeployment/restart) can be perform using **ansible** playbook - so there is a need to have ansible installed.
Prepared release procedure was tested on AmazonLinux2023 (https://github.com/amazonlinux/amazon-linux-2023) 

### Deployment

* Deploy on local machine
```bash
cd ansible
ansible-playbook -i ./inventory ./app_weathercast.yml -t prepare_instance,deploy
```
* Deploy on remote machine (change the IP address in ansible/inventory for the relevant one)
```bash
cd ansible
ansible-playbook -i ./inventory ./app_weathercast.yml -t prepare_instance,deploy -e e_hosts=aws_docker --key path_to_private_ssh_key
```
### Undeployment

* Undeploy from local machine
```bash
cd ansible
ansible-playbook -i ./inventory ./app_weathercast.yml -t udeploy
```
* Undeploy from remote machine (change the IP address in ansible/inventory for the relevant one)
```bash
cd ansible
ansible-playbook -i ./inventory ./app_weathercast.yml -t undeploy -e e_hosts=aws_docker --key path_to_private_ssh_key
```

### Restart the application
* Restart on local machine
```bash
cd ansible
ansible-playbook -i ./inventory ./app_weathercast.yml -t restart
```
* Restart on remote machine (change the IP address in ansible/inventory for the relevant one)
```bash
cd ansible
ansible-playbook -i ./inventory ./app_weathercast.yml -t restart -e e_hosts=aws_docker --key path_to_private_ssh_key
```
### Tags available for ansible playbook
```bash
-t prepare_instance - install docker and docker compose on managed machine
-t deploy - deploy the application
-t restart - restart the application
-t rebuild - rebuild the application (enforce rebuilding docker images)
-t undeploy - undeploy the application
```

### External variables available for ansible playbook
```
-e e_user=value - define the username (system account) used during deployment (ec2-user - default value)
-e e_hosts=value - name the of host group defined in inventory (localhost - default value)
-e e_FQDN=value - under this FQDN application will be visible (the relevant DNS record poiting proper IP has to exist) (weathercast.dwolnicki.net - defaule value) 
```

### Future Improvements
In order to provide with
* High Avaialbility
* Scalability
* Easiest deployment (Eg. via HELM)
I is worth to migrate prod application to K8S :)


