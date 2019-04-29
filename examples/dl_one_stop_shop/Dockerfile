# ============================================================================
# module list 
# ----------------------------------------------------------------------------
# python 3.6.7
# pytorch 1.0
# ============================================================================
FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu16.04
ENV KERAS_BACKEND="tensorflow"
RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="pip --no-cache-dir install --upgrade" && \
    GIT_CLONE="git clone --depth 10" && \
    rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
	   /etc/apt/sources.list.d/nvidia-ml.list && \
    apt-get update && \
# ============================================================================
# tools
# ----------------------------------------------------------------------------
   DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
	apt-utils \        
	build-essential \
        ca-certificates \
        cmake \
        wget \
        git \
        vim \
        unixodbc-dev \
#        unixodbc-dev-utf16 \
        freetds-dev \
	nano \
	locales \
	tzdata \
	software-properties-common \
	libssl-dev \
	ncdu \
	htop \
	screen \
	&&\

# ============================================================================
# python
# ----------------------------------------------------------------------------
   DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        software-properties-common \
        && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        python3.6 \
        python3.6-dev \
        python3.6-tk \
        && \
    wget -O ~/get-pip.py \
        http://bootstrap.pypa.io/get-pip.py && \
    python3.6 ~/get-pip.py && \
    ln -s /usr/bin/python3.6 /usr/local/bin/python3 && \
    ln -s /usr/bin/python3.6 /usr/local/bin/python && \
    $PIP_INSTALL \
        setuptools \
        && \
    $PIP_INSTALL \
        numpy \
        scipy \
        pandas \
	pandas-datareader \
        scikit-learn \
	scikit-image \
        matplotlib \
	pillow \
	requests \
	h5py \
        Cython \
	cupy-cuda100 \
        tqdm \
        gensim \
        ipython \
        jupyter \
        umap-learn \
        hdbscan \
        seaborn \
        bokeh \
        allennlp \
        spacy \
        visdom \
        sqlalchemy \
        pymssql \
        pyodbc \
	nltk \
	bpemb \
	networkx \
	lime \
	jupyterlab \
	jupyterhub \
	hebrew_tokenizer \
	flask \
	flask_cors \
	flask_ipywidgets \
	catboost \
	ipywidgets \
	subword-nmt \
	sentencepiece \
        && \

# ============================================================================
# pytorch 
# ---------------------------------------------------------------------------
    $PIP_INSTALL \
        torch torchvision \
        git+https://github.com/pytorch/tnt.git@master \
        torchtext \
	tensorboardX \
	pytorch_pretrained_bert \
	&& \

# ============================================================================
# Tensorflow
# ----------------------------------------------------------------------------
    $PIP_INSTALL  \
	tensorflow \
	tensorflow-gpu \
	keras \
	unicodecsv \
	distance \
	jellyfish \
	&& \

# ============================================================================
# config & cleanup
# ----------------------------------------------------------------------------
    ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*


# ============================================================================
# ELMo
# ----------------------------------------------------------------------------
RUN wget https://github.com/HIT-SCIR/ELMoForManyLangs.git

# ============================================================================
# Download spacy models
# ----------------------------------------------------------------------------
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
RUN python -m spacy download en_core_web_sm
RUN python -m spacy download en
RUN python -m nltk.downloader perluniprops nonbreaking_prefixes brown
RUN python -c 'from visdom.server import download_scripts; download_scripts()' 
RUN python -c 'from bpemb import BPEmb; BPEmb(lang="he", dim=300, vs=200000)'

# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fonts-dejavu \
    gfortran \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# R
RUN apt-get update
RUN apt-get install -y r-base
COPY packages.r /root/packages.r

RUN ln -s /bin/tar /bin/gtar
RUN Rscript /root/packages.r

WORKDIR /root/

RUN apt install -y openssh-server

RUN echo 'root:123456' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

RUN service ssh start

EXPOSE 22
ENTRYPOINT service ssh start && /bin/bash

