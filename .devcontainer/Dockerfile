# 使用 Ubuntu 20.04 作为基础镜像，兼容性最好
FROM ubuntu:20.04

# 避免交互式前端报错
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Shanghai

# 1. 替换源（可选，GitHub Actions 网络通常很好，使用官方源即可）
# 2. 安装 Padavan 编译所需的依赖库
# 注意：这里包含了两套源码(Hanwckf/MeIsReallyBa)所需的公共依赖
RUN apt-get update && apt-get install -y \
    build-essential \
    libtool-bin \
    gperf \
    gawk \
    flex \
    bison \
    nano \
    xxd \
    fakeroot \
    kmod \
    cpio \
    git \
    vim-common \
    libncurses5-dev \
    libncurses5 \
    python3-docutils \
    autopoint \
    texinfo \
    pkg-config \
    unzip \
    automake \
    autoconf \
    gettext \
    libtirpc-dev \
    libgmp-dev \
    libmpc-dev \
    libmpfr-dev \
    autopoint \
    python2 \
    python3 \
    python3-pip \
    sudo \
    curl \
    wget \
    bc \
    rsync \
    locales \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 3. 设置语言环境 (防止编译时出现非法字符错误)
RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

# 4. 配置 Python 环境
# 旧版工具链可能调用 `python`，这里将其指向 python3 (或者 python2，视具体源码需求而定，Padavan通常 python3 即可)
RUN ln -sf /usr/bin/python3 /usr/bin/python

# 5. 准备工作目录
WORKDIR /workspace

# 6. 预创建工具链目录 (避免权限问题)
RUN mkdir -p /opt/rt-n56u/toolchain-mipsel && chmod -R 777 /opt

# 7. (可选) 添加非 root 用户，某些工具链禁止 root 编译
# GitHub Actions 默认是 root，但为了安全和兼容性，可以预留配置
# 这里保持 root 运行以便于 CI/CD 流程简化
