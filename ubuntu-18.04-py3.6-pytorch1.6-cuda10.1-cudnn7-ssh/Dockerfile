# BASE IMAGE
FROM nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04

# LABEL MAINTAINER
LABEL maintainer="seliangchen@163.com"

SHELL ["/bin/bash","-c"]

WORKDIR /tmp

# copy安装文件
COPY Python-3.6.10.tar.xz /tmp

ENV DEBIAN_FRONTEND=noninteractive

# 设置 root 密码
RUN echo 'root:password' | chpasswd \
# 安装openssh-server 并配置
  && apt-get update && apt-get -y install openssh-server \
  && sed -i 's/#PermitRootLogin yes/PermitRootLogin yes/g' /etc/ssh/sshd_config \
  && sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config \
  && mkdir /var/run/sshd 

# 安装python依赖包
RUN apt-get -y install build-essential python-dev python-setuptools python-pip python-smbus \
  && apt-get -y install build-essential libncursesw5-dev libgdbm-dev libc6-dev \
  && apt-get -y install zlib1g-dev libsqlite3-dev tk-dev \
  && apt-get -y install libssl-dev openssl \
  && apt-get -y install libffi-dev \
  && apt-get -y install python-lzma \
  && apt-get -y install liblzma-dev \
  && apt-get -y vim

# 安装python 3.6.10
RUN mkdir -p /usr/local/python3.6 \
  && tar xvf Python-3.6.10.tar.xz \
  && cd Python-3.6.10 \
  && ./configure --prefix=/usr/local/python3.6 \
  && make altinstall \
# 建立软链接
  && ln -snf /usr/local/python3.6/bin/python3.6 /usr/bin/python3 \
  && ln -snf /usr/local/python3.6/bin/pip3.6 /usr/bin/pip3

# 安装pytorch
RUN mkdir ~/.pip && echo -e '[global] \nindex-url = https://mirrors.aliyun.com/pypi/simple/' >> ~/.pip/pip.conf \
  && pip3 install --upgrade pip \
  && pip3 install torch==1.6.0+cu101 torchvision==0.7.0+cu101 -f https://download.pytorch.org/whl/torch_stable.html \
# 清理copy的安装文件
  && apt-get clean \
  && rm -rf /tmp/* /var/tmp/*
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]