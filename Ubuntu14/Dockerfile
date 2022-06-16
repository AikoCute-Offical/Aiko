FROM ubuntu:14.04

RUN apt-get update && apt-get -y upgrade
# RUN apt-get install -y software-properties-common
# RUN DEBIAN_FRONTEND=noninteractive apt-get -y install python-software-properties

# RUN DEBIAN_FRONTEND=noninteractive add-apt-repository ppa:openjdk-r/ppa 
RUN apt-get update

# Install the following utilities (required by aosp build)
# RUN apt-get install -y git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev
# RUN apt-get install -y gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev
# RUN apt-get install -y x11proto-core-dev libx11-dev lib32readline-gplv2-dev lib32z1-dev
# RUN apt-get install -y libxml-simple-perl libc6-dev libgl1-mesa-dev tofrodos
# RUN apt-get install -y python-markdown libxml2-utils xsltproc openjdk-7-jdk bc mkisofs python-imaging

# do some fancy footwork to create a JAVA_HOME that's cross-architecture-safe
# RUN ln -svT "/usr/lib/jvm/java-7-openjdk-$(dpkg --print-architecture)" /docker-java-home
# ENV JAVA_HOME /docker-java-home

RUN apt-get update && apt-get install -y git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip python openjdk-7-jdk
RUN curl -o jdk8.tgz https://android.googlesource.com/platform/prebuilts/jdk/jdk8/+archive/master.tar.gz \
 && tar -zxf jdk8.tgz linux-x86 \
 && mv linux-x86 /usr/lib/jvm/java-8-openjdk-amd64 \
 && rm -rf jdk8.tgz

RUN curl -o /usr/local/bin/repo https://storage.googleapis.com/git-repo-downloads/repo \
 && chmod a+x /usr/local/bin/repo

# Install Jfrog cli utility to deploy artifacts
# RUN cd /usr/bin; curl -fL https://getcli.jfrog.io | sh
# RUN chmod 755 /usr/bin/jfrog

# Create a non-root user that will perform the actual build
RUN id doraemon 2>/dev/null || useradd --uid 1000 --create-home doraemon
RUN echo "doraemon ALL=(ALL) NOPASSWD: ALL" | tee -a /etc/sudoers

# Default sh to bash
# RUN echo "dash dash/sh boolean false" | debconf-set-selections
# RUN DEBIAN_FRONTEND=noninteractive dpkg-reconfigure dash

# Disable Host Key verification.
RUN mkdir -p /home/doraemon/.ssh
RUN echo -e "Host *\n\tStrictHostKeyChecking no\n" > /home/doraemon/.ssh/config
RUN chown -R doraemon:doraemon /home/doraemon/.ssh

# RUN sudo git clone https://github.com/akhilnarang/scripts /scripts

# RUN cd /scripts && bash setup/android_build_env.sh && bash setup/install_android_sdk.sh

# Git
RUN git config --global user.email "polarisdp@gmail.com"
RUN git config --global user.name "dopaemon"
RUN git config --global color.ui false

USER doraemon
ENV USER doraemon
WORKDIR /home/doraemon
# CMD "/bin/bash"

ENTRYPOINT chroot --userspec=$(cat /home/doraemon):$(cat /home/doraemon) / /bin/bash -i
