# MicroShift OSBuild Automation

This repository is a a set of playbooks to automate the OSBuild host preparation.

The repository content is based on the workflow described in this [blogpost](https://cloud.redhat.com/blog/meet-red-hat-device-edge-with-microshift).
Thanks to [Ben Schmaus](https://github.com/schmaustech) for the detailed informations.

## Creation of RHEL 8 build node VM

TODO

## Preparation of the build node
Execute the `prepare_build_node.yaml` to configure the build node for MicroShift builds.
```
$ ansible-playbook -i inventory.yaml prepare_build_node.yaml
```

The preparation process will take quite a long time to sync the Microshift local repo, be patient.

## Build the Microshift release

TODO

## Authors
Gianni Salinetti <gsalinet@redhat.com>
