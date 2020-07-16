Bootstrap: docker
From: nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04

%labels
MAINTAINER Timothy R. Fallon 

%files

%environment
 PACKAGE_VERSION=4.0.11
 BUILD_PACKAGES="wget apt-transport-https"
 DEBIAN_FRONTEND=noninteractive

%post
    PACKAGE_NAME=ont_guppy_${PACKAGE_VERSION}-1~bionic_amd64.deb

    apt-get update
    apt-get install locales
    locale-gen "en_US.UTF-8"
    dpkg-reconfigure locales
    apt-get install --yes $BUILD_PACKAGES \

    cd /tmp
    wget -q https://mirror.oxfordnanoportal.com/software/analysis/${PACKAGE_NAME}
    dpkg -I ${PACKAGE_NAME} ##Print some information about the package dependencies
    dpkg -i --ignore-depends=nvidia-driver-418,libcuda1-418 /tmp/${PACKAGE_NAME}
    rm -f *.deb
    apt-get autoremove --purge --yes
    apt-get clean
    rm -rf /var/lib/apt/lists/*
    
%runscript
