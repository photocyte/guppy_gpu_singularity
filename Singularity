Bootstrap: docker
From: nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04

%labels
MAINTAINER Timothy R. Fallon 

%files

%environment

%post
    PACKAGE_VERSION=4.0.11
    BUILD_PACKAGES="wget apt-utils apt-transport-https"
    DEBIAN_FRONTEND=noninteractive
    PACKAGE_NAME=ont_guppy_${PACKAGE_VERSION}-1~bionic_amd64.deb

    apt-get update
    apt-get install --yes $BUILD_PACKAGES
    apt-get install --yes locales
    locale-gen "en_US.UTF-8"
    dpkg-reconfigure locales
    cd /tmp
    apt-get install --yes libzmq5 \
	libhdf5-cpp-100 \
	libboost-atomic1.65.1 \
        libboost-chrono1.65.1 \
        libboost-date-time1.65.1 \
        libboost-filesystem1.65.1 \
        libboost-iostreams1.65.1 \
        libboost-program-options1.65.1 \
        libboost-regex1.65.1 \
        libboost-system1.65.1 \
        libboost-log1.65.1
    wget -q https://mirror.oxfordnanoportal.com/software/analysis/${PACKAGE_NAME}
    dpkg -I ${PACKAGE_NAME} ##Print some information about the package dependencies
    dpkg -i --ignore-depends=nvidia-driver-418,libcuda1-418 /tmp/${PACKAGE_NAME}
    rm -f *.deb
    apt-get autoremove --purge --yes
    apt-get clean
    rm -rf /var/lib/apt/lists/*
    
%runscript
