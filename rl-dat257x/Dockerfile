# docker build -t rl-dat257x .
# docker run -ti -p 8888:8888 rl-dat257x
# docker run -ti -p 8888:8888 -v ~/host/directory:/docker/directory rl-dat257x

FROM ubuntu:16.04

RUN apt-get update && apt-get install -y --no-install-recommends \
            build-essential \
            sudo \
            wget \
            bzip2 \
            ca-certificates \
            libglib2.0-0 \
            libxext6 \
            libsm6 \
            libxrender1 \
            git \
            mercurial \
            subversion \
            cmake \
            curl \
            sed \
            dpkg \
            python-dev \
            python3-dev \
            python-pip \
            python3-pip \
            python-virtualenv \
            python-wheel \
            dh-autoreconf \
            python-setuptools \
            python3-setuptools && \            
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    pip2 install --upgrade pip && \
    pip3 install --upgrade pip

RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/archive/Anaconda3-4.1.1-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

RUN adduser --disabled-password --gecos '' docker && \
    adduser docker sudo && \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER docker
RUN sudo -H /opt/conda/bin/python3.5 -m pip install --upgrade pip && \
    sudo -H /opt/conda/bin/python3.5 -m pip install gym && \
    sudo -H /opt/conda/bin/python3.5 -m pip install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.3-cp35-cp35m-linux_x86_64.whl && \
    sudo -H /opt/conda/bin/python3.5 -m pip install chainer

EXPOSE 8888

RUN mkdir ~/notebooks
WORKDIR /home/docker/notebooks
ENV PATH /opt/conda/bin:$PATH    
CMD sh -c "jupyter notebook --no-browser --ip=*"
