## Overview
### Repository Overview
This repository provides Ansible playbooks and configuration files for deploying and managing Red Hat Ansible Automation Platform (RHAAP) on OpenShift (OCP) using the Ansible Automation Platform Operator. It is designed to test active/passive deployments and disaster recovery (DR) concepts with external PostgreSQL databases from a single Openshift instance. 

### Features
- Deploy the AAP Operator and Automation Controller across multiple namespaces on a single OpenShift cluster/instance
- Automated secret creation including admin passwords and encryption keys
- External database secret creation to enable testing of HA/DR concepts
- Customizable YAML files for different deployments of Automation Controller

### Prerequisites 
- oc binaries installed locally on the host running the playbook
- Run the oc login command to authenticate to your Openshift cluster.
- Configure external DB cluster, configure awx user and db according to AAP docs.
- Update the yml files in the files/ directory.
- Install the redhat.openshift Ansible collection. 

### Updating files
- aap_operator.yml: Controls Namespaces, OperatorGroup and the subscription for AAP Operator.
- aap-primary-deployment.yml: AutomationController CR for primary controller.
- aap-secondary-deployment.yml: AutomationController CR for DR/backup controller.
- controller-edb-01-admin-password.yml: Admin password, you can have the controller default deployment deploy one for you then copy it or use an existing admin password. Regardless they need to be the same in both namespaces.
- controller-edb-02-admin-password.yml: Same as above but for the 'DR' namespace.
- secret-key.yml files: This is the k8s secret that needs to be the same on both the primary and DR namespaces.
- external-postgres-configuration secrets: For this to work properly you should have a primary node configured for site 1 and a replica/standby for site 2. Site 01 will point to the primary IP, and site 02 will point to the standby IP both using the same database and user/password.

### Run the playbook
- First: ensure you login to the openshift cluster with the oc command.
- execute `ansible-playbook deploy-operator.yml`. 
