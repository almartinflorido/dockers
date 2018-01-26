# docker build -t caffe_deep_dreams .
# docker run -ti -p 8888:8888 rl-dat257x
# docker run -ti -p 8888:8888 -v ~/host/directory:/docker/directory rl-dat257x

FROM ubuntu:16.04

RUN apt-get update && apt-get install -y --no-install-recommends \
            build-essential \
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
    wget --quiet https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

RUN pip install gym && \
    pip3 install gym && \
    pip3 install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.3-cp35-cp35m-linux_x86_64.whl && \
    pip3 install chainer

EXPOSE 8888

RUN useradd -ms /bin/bash user
USER user
RUN mkdir ~/notebooks
WORKDIR /home/user/notebooks
ENV PATH /opt/conda/bin:$PATH    
CMD ["jupyter", "notebook", "--no-browser", "--ip=0.0.0.0"]
