# bcl_use_case2

## Action plan:

Create the following roles:

 - RHEL custom ISO creation > we have it
 - Preparation of Windows ISO with WinRM enabled > Ale has something
 - Create VMware templates from custom ISO's > we cannot test it at the moment
 - Create VM from templates > Pablo
 - Configure web server > Amaya

# Using the playbooks in this repo

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

## Create a custom execution environment

Install ansible-builder:

```
pip install ansible-builder
```
Navigate to the "execution-environment" directory of this repo and run:

```
ansible-builder build -f execution-environment.yml -v 3 -t bcl_uc2:5
```

We've built our image with community content and uploaded to ghcr.io, but we highly recommend you to build your build your own image with secure and stable content coming from Red Hat subscriptions. Once you've built it change the image reference in ansible-navigator.yml.

In case you still want to use our unsecure and unstable image you can pull it from:

```
podman pull ghcr.io/red-hat-emea-portfolio-ssa/bcl_uc2:5
```