# Rocky9

This will help building a driver image for Rocky9. Initially copied from cento8 folder.

Few things were changed

in Dockerfile:

- replace base image cuda:11.4.1-base-centos8 -> cuda:12.8.1-base-rockylinux9
- remove old kernel-headers


in nvidia-driver:

- added --enablerepo=elrepo to dnf to install new kernel
- added tail -n 1 , el kernel outputs 2 lines instead of 1



## build

- Run on a gpu node

```

export NVIDIA_DRIVER_VERSION=550.144.03
nerdctl build \
     --network host \
     -t "nvidia/driver:$NVIDIA_DRIVER_VERSION-rocky9.4" \
     --build-arg "NVIDIA_DRIVER_VERSION=$NVIDIA_DRIVER_VERSION" \
     -f Dockerfile .
```

## Deploying

Deploy the operator helm chart with the values for `NVIDIA_DRIVER_VERSION` - as an example:

```shell
export NVIDIA_DRIVER_VERSION=550.144.03
helm install --generate-name \
     -n gpu-operator --create-namespace \
     nvidia/gpu-operator \
     --set driver.repository=nvidia \
     --set driver.version=$NVIDIA_DRIVER_VERSION
```
