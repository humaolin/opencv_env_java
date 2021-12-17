FROM ubuntu:20.04

SHELL ["/bin/bash", "-c"]

# this line is to prevent error of "debconf: delaying package configuration"
ENV DEBIAN_FRONTEND noninteractive
ENV TZ=Asia/Shanghai

# install softwares and system tools in need
RUN sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && sed -i '/#/d' /etc/apt/sources.list \
    && sed -i '/^$/d' /etc/apt/sources.list \
    && echo "deb http://security.ubuntu.com/ubuntu xenial-security main" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ xenial universe" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/ubuntu/ bionic universe" >> /etc/apt/sources.list \
    && echo "APT::Acquire::Retries \"50\";" > /etc/apt/apt.conf.d/80-retries \
    && echo -e "\n----- Adding utilites -----" \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        apt-utils \
        iproute2 \
        iputils-ping \
        lcov \
        net-tools \
        openssh-server \
        pkg-config \
        rsync \
        software-properties-common \
        sshpass \
        telnet \
        unzip \
        vim \
        wget \
        curl \
        zip \
    && echo -e "\n----- Adding runtime dependencies -----" \
    && apt-get install -y --no-install-recommends \
        libgl1 \
        libglib2.0-0 \
        libtbb2 \
        libjpeg8 \
        libpng16-16 \
        libtiff5 \
        libdc1394-22 \
        libfreetype6 \
        libharfbuzz0b \
        libxkbcommon-x11-0 \
        libgomp1 \
        libfontconfig1 \
        libjasper1 \
        libgssapi-krb5-2 \
        libsm6 \
        libxrender1 \
        libxext-dev \
        libpulse0 \
        libpulse-mainloop-glib0 \
        uuid-dev \
        sysstat \
     && echo -e "\n----- Adding center-image dependencies -----" \
        && apt-get install -y --no-install-recommends \
        gfortran libopenblas-dev  libblas-dev  liblapack-dev \
    	libatlas-base-dev  libopenblas-base \
    && echo -e "\n----- Adding development dependencies -----" \
    && apt-get install -y --no-install-recommends \
        cmake \
        libjasper-dev \
    && echo -e "\n--- Adding OpenCV dependencies --- part 1" \
    && apt-get install -y --no-install-recommends \
        python-dev \
        python-numpy \
        python-pip-whl \
        python3-dev \
        python3-numpy \
        python3-pip \
        libtbb2 \
        libtbb-dev \
        libjpeg-dev \
        libpng-dev \
        libtiff-dev \
        libdc1394-22-dev \
    && echo -e "\n--- Adding OpenCV dependencies --- part 2" \
    && apt-get install -y --no-install-recommends \
        libv4l-dev \
        libgtk2.0-dev \
        libgtk-3-dev \
        libfreetype6-dev \
        libharfbuzz-dev \
    && echo -e "\n--- Adding OpenCV dependencies --- part 3" \
    && apt-get install -y --no-install-recommends \
        libatlas-base-dev \
        gfortran \
        libhdf5-serial-dev \
        libgl-dev \
    && echo -e "\n--- Adding Qt dependencies ---" \
    && apt-get install -y --no-install-recommends \
        build-essential \
        libfontconfig1 \
        mesa-common-dev \
        libgl1-mesa-dev \
    && echo -e "\n--- Adding java ---" \
    && apt-get install -y --no-install-recommends \
        bash-completion \
        openjdk-11-jdk \
        ant \
        maven \
    && apt-get install -y openssl \
        libssl-dev \
    && echo -e "\n--- Adding Git and Git LFS ---" \
    && curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash \
    && apt-get install -y --no-install-recommends \
        git \
        git-lfs \
    && apt-get -y clean \
    && apt-get -y autoremove --purge \
    && rm -rf /var/lib/apt-get/lists/* \
    && sed -i "/StrictHostKeyChecking/c StrictHostKeyChecking no" /etc/ssh/ssh_config \
    && echo service ssh start >> ${HOME}/.bashrc


# install development dependencies
RUN mkdir -p ${HOME}/.config/pip \
    && echo "[global]" > ${HOME}/.config/pip/pip.conf \
    && echo "index-url=https://mirrors.aliyun.com/pypi/simple/" >> ${HOME}/.config/pip/pip.conf



# set java environment variables
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
ENV JRE_HOME=${JAVA_HOME}/jre
ENV CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
ENV PATH=${PATH}:${JAVA_HOME}/bin



RUN  apt-get remove --purge --auto-remove -y && ln -snf /usr/share/ant/bin/ant /bin/ant && rm -rf /var/lib/apt/lists/* \
        \
        && if [ -n "$tempDir" ]; then \
            apt-get purge -y --auto-remove \
            && rm -rf "$tempDir" /etc/apt/sources.list.d/temp.list; \
        fi
