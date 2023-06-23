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

## Create a VMware_rest execution environment

Install ansible-builder:

´´´
pip install ansible-builder

´´´
Navigate to the vmware_rest-ee directory and run:

```
ansible-builder build -f execution-environment.yml -t vmware_rest-ee:1 -v 3
```

We've built our image with community content and uploaded to ghcr.io, but we highly recommend you to build your build your own image with secure and stable content coming from Red Hat subscriptions. Once you've built it change the image reference in ansible-navigator.yml.

In case you still want to use our unsecure and unstable image you can pull it from:

´´´
podman pull ghcr.io/red-hat-emea-portfolio-ssa/bcl_use_case2:vmware_rest_ee
´´´