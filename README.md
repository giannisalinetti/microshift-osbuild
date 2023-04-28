# MicroShift OSBuild Automation

This repository is a a set of playbooks to automate the OSBuild host preparation.

The repository content is based on the workflow described in this [blogpost](https://cloud.redhat.com/blog/meet-red-hat-device-edge-with-microshift).
Thanks to [Ben Schmaus](https://github.com/schmaustech) for the detailed informations.

## Creation of RHEL 8 build node VM

TODO

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
