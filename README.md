# MicroShift OSBuild Automation

This repository is a a set of playbooks to automate the OSBuild host preparation.

The repository content is based on the workflow described in this [blogpost](https://cloud.redhat.com/blog/meet-red-hat-device-edge-with-microshift).
Thanks to [Ben Schmaus](https://github.com/schmaustech) for the detailed informations.

## Creation of RHEL 8 build node VM

To deploy the VM on Linux with KVM/Libvirt, download the RHEL 8.7 qcow2 image file from the official [RHEL download page](https://access.redhat.com/downloads/content/479/ver=/rhel---8/8.7/x86_64/product-software).

With the `virt-customize` tool, customize the image to disable the cloud-init service and add a root password:
```
$ virt-customize -a <qcow2 image file name> --root-password password:<password> --uninstall cloud-init
```

To import the VM use the `virt-manager` console or the `virt-install` CLI. The following example 
shows how to import the VM from command line:
```
$ sudo  virt-install \
  --name rhel8-osbuild \
  --memory 4096 \
  --vcpus 2 \
  --disk /path/to/imported/disk.qcow2,size=20 \
  --import \
  --os-variant rhel8.7
```
  
  > **_IMPORTANT_** The imported disk must have a total size of at least 20GiB and a minimum available size of 16GiB to allow sufficiente space for the builds.

## Customize variables

The `vars.yaml` file contains all the variables used in the project.
Customize paths, passwords, secrets and tokens with your values.

### Generate offline token
First, obtain a valid offline token from Red Hat. To generate it, go to https://access.redhat.com/management/api and 
click on the **GENERATE TOKEN** button.

Set the `api_token` variable in the `vars.yaml` file with the obtained token.

### Update pull secret
In order to correctly pull images, Microshift needs a pull secret that is injected in the image.
To inject a valid pull secret, download it from the [Red Hat Hybrid Cloud Console](https://console.redhat.com/openshift) and assigh it to the `pull_secret` variable.


## Full build lifecycle
The `main.yaml` file recalls all the necessary playbooks, in the exact order, to prepare the build node,
build the Microshift container image and create the final RHDE ISO.
```
$ ansible-playbook -i inventory.yaml main.yaml
```

## Preparation of the build node
To prepare the build node launch the `00-prepare-build-node.yaml`. This playbook configures the build node for MicroShift builds.
```
$ ansible-playbook -i inventory.yaml 00-prepare-build-node.yaml
```

The preparation process will take quite a long time to sync the Microshift local repo, be patient.

## Build the Microshift release
To build the Microshift release in a container image launche the `01-build-microshift-image.yaml`.
```
$ ansible-playbook -i inventory.yaml 01-build-microshift-image.yaml
```

## Create the final RHDE ISO
To create a bootable, ZTP capable RHDE ISO, launch the `02-create-rhde-iso.yaml` playbook:
```
$ ansible-playbook -i inventory.yaml 02-create-rhde-iso.yaml
```


## Authors
Gianni Salinetti <gsalinet@redhat.com>
