# metal-block-storage

![](https://img.shields.io/badge/Stability-Maintained-green.svg)

This repository is [Maintained](https://github.com/packethost/standards/blob/master/maintained-statement.md) meaning that this software is supported by Equinix Metal and its community - available to use in production environments.

----
## Quick overview
This utility/helper script is made available to assist in  the attachment and detachment of Equinix Metal block storage volumes and is currently in testing. Use at your own risk and always have a backup/snapshot just in case!

# Availability
The metal-block storage helper scripts are pre-installed on the following Equinix Metal operating systems:
* CentOS
* Ubuntu
* Debian
* OpenSUSE
* Redhat Enterprise Linux
* SLES 12 SP3

# Features
* Attach block storage block devices
* Detach block storage block devices
* Configure multipath for HA
* Validate block device creation

# Dependencies

**Ubuntu/Debian**:

    apt-get install open-iscsi multipath-tools jq


**CentOS**:

    yum -y install iscsi-initiator-utils device-mapper-multipath jq

## Installation

**Using git:**

    sudo su -
    git clone git@github.com:packethost/metal-block-storage.git
    cp ./metal-block-storage/metal-block-storage-* /usr/bin/
    chmod u+x /usr/bin/metal-block-storage-*

**or**

**Using wget**

    sudo su -
    wget -O /usr/bin/metal-block-storage-attach https://raw.githubusercontent.com/packethost/metal-block-storage/master/metal-block-storage-attach
    wget -O /usr/bin/metal-block-storage-detach https://raw.githubusercontent.com/packethost/metal-block-storage/master/metal-block-storage-detach
    chmod u+x /usr/bin/metal-block-storage-*

## Usage

**Attach a volume**

1. Create a volume under the Storage tab in your Equinix Metal portal account
2. Click on the storage volume, select the server you wish to attach the volume to and click on Attach.
3. Execute metal-block-storage-attach from within the OS on the server in question
4. Partition the block device at /dev/mapper/\{volume\_name\_here\} disk using parted, fdisk, etc
5. Make a filesystem on the block device
6. Mount the block device on the mount point of you choice

Example :

    [root@cent7-pbs-client dlaube]# metal-block-storage-attach
    Block device /dev/mapper/volume-9ab99df5 is available for use
    Block device /dev/mapper/volume-7eab8fc1 is available for use

**IMPORTANT NOTE:** Consider using the "-m queue" flag when attaching storage to configure multipath queuing. If block storage becomes unreachable, the option "fail" results in FS read-only and "queue" will keep IO in memory buffer until reachable. See "-h" for usage details.



**Detach a volume**

1. Unmount the filesystem on any block storage volume that may be in use
2. Execute metal-block-storage-detach from within the OS on the server in question
3. Click on the storage volume from the storage tab within your Equinix Metal project and click on Detach

Example:

    [root@cent7-pbs-client dlaube]# metal-block-storage-detach

