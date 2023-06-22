# Using this playbooks for VMware

You need to export two variables into your environment:

```
export VMWARE_HOST=<your-vcenter-fqdn>
export VMWARE_USER=<your-vcenter-user>
export VMWARE_PASSWORD=<your-vcenter-password>
export VMWARE_VALIDATE_CERTS=<true-or-false>
```
The ansible-navigator.yml file in this repo is already configured to pass those env variables to your execution environment. This way you don't need to hardcode your passwords.

Remember to modify the vars.yml file with your values and then run the playbook:

```
ansible-navigator run pb-create_vmware_template.yml -e @vars.yml
```