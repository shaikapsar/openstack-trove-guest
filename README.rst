## Image elements for building Trove guest images

sudo apt-get install qemu-utils python-virtualenv -qy
virtualenv .venv
source .venv/bin/activate
pip install dib-utils pyyaml

export DIB_CLOUD_INIT_ETC_HOSTS=true
export DIB_CLOUD_INIT_DATASOURCES="Ec2, ConfigDrive, OpenStack"

export DIB_DEV_USER_USERNAME=cloudapp
export DIB_DEV_USER_PASSWORD=w8EmsJXTMaG0e97NK8lM
export DIB_DEV_USER_SHELL=/bin/bash
export DIB_DEV_USER_PWDLESS_SUDO=true

export DIB_DEV_USER_AUTHORIZED_KEYS=/home/sa709c/.ssh/authorized_keys

export ELEMENTS_PATH=/home/sa709c/dib/diskimage-builder/elements:/home/sa709c/dib/trove-integration/scripts/files/elements:/home/sa709c/dib/tripleo-image-elements/elements:/home/sa709c/dib/openstack-trove-guest/elements

disk-image-create  -a amd64 -o ubuntu-mysql ubuntu  openstack-ubuntu-guest


glance image-create  --name ubuntu-element --disk-format  qcow2 --container-format bare --file ubuntu-mysql.qcow2 --is-public True

ip netns exec qdhcp-3c4ad269-751b-494d-b035-7d27cc38a368 ssh ks@10.0.0.119