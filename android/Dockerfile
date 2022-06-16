FROM ubuntu:20.04

LABEL maintainer.name="ROOT team"
LABEL maintainer.email="root-dev@cern.ch"

ENV LANG=C.UTF-8

ARG ROOT_BIN=root_v6.22.06.Linux-ubuntu20-x86_64-gcc9.3.tar.gz

WORKDIR /opt

COPY packages packages

RUN apt-get update -qq \
 && ln -sf /usr/share/zoneinfo/UTC /etc/localtime \
 && apt-get -y install $(cat packages) wget\
 && rm -rf /var/lib/apt/lists/*\
 && wget https://root.cern/download/${ROOT_BIN} \
 && tar -xzvf ${ROOT_BIN} \
 && rm -f ${ROOT_BIN} \
 && echo /opt/root/lib >> /etc/ld.so.conf \
 && ldconfig

ENV ROOTSYS /opt/root
ENV PATH $ROOTSYS/bin:$PATH
ENV PYTHONPATH $ROOTSYS/lib:$PYTHONPATH
ENV CLING_STANDARD_PCH none

CMD ["root", "-b"]

# Install dependencies
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install bison repo libssl-dev build-essential curl flex git gnupg gperf liblz4-tool libncurses5-dev libsdl1.2-dev libxml2 libxml2-utils lzop pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev build-essential kernel-package libncurses5-dev bzip2 git python sudo gcc g++ openssh-server tar gzip ca-certificates -y

RUN apt-get purge openjdk-8-jdk openjdk-8-jre openjdk-11-jdk openjdk-11-jre -y

RUN apt-get install openjdk-8-jdk openjdk-8-jre -y

RUN apt-get install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev libwxgtk3.0-dev -y

RUN rm -rf /var/lib/apt/lists/*

# Link Timezone
RUN ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime

# Add User
# Why? Well for avoid something wrong
# I've seen some notes for not using root when build
RUN useradd -ms /bin/bash doraemon
RUN echo "doraemon ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER doraemon
WORKDIR /home/doraemon

# Git
RUN git config --global user.email "polarisdp@gmail.com"
RUN git config --global user.name "dopaemon"
RUN git config --global color.ui false



RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
        bison \
        ca-certificates \
        flex \
        g++ \
        gcc \
        git \
        libprotobuf-dev \
        libprotobuf9v5 \
        make \
        pkg-config \
        protobuf-compiler \
    && git clone --depth=1 --branch=2.5 https://github.com/google/nsjail.git /nsjail \
    && cd /nsjail \
    && make \
    && mv /nsjail/nsjail /usr/sbin \
    && apt-get remove --purge -y \
        bison \
        ca-certificates \
        flex \
        g++ \
        gcc \
        git \
        libprotobuf-dev \
        make \
        pkg-config \
        protobuf-compiler \
        $(apt-mark showauto) \
    && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/* /nsjail


# RUN mkdir -p /home/doraemon/bin

# RUN curl https://storage.googleapis.com/git-repo-downloads/repo > /home/doraemon/bin/repo

# RUN chmod a+x /home/doraemon/bin/repo

# RUN echo "if [ -d "$HOME/bin" ] ; then" >> /home/doraemon/.profile
# RUN echo "    PATH="$HOME/bin:$PATH"" >> /home/doraemon/.profile
# RUN echo "fi" >> /home/doraemon/.profile

RUN sudo git clone https://github.com/akhilnarang/scripts /scripts

RUN cd /scripts && bash setup/android_build_env.sh && bash setup/install_android_sdk.sh

CMD ["/usr/sbin/nsjail", "-Mo", "--user", "31337", "--group", "31337", "--chroot", "/", "--", "/bin/sh"]




