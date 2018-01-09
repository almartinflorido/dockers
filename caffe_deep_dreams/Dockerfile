# nvidia-docker build -t caffe_deep_dreams .
# nvidia-docker run -ti -p 8888:8888 caffe_deep_dreams
# nvidia-docker run -ti -p 8888:8888 -v ~/ubuntu_cafe/deepdream:/opt/deep_dream caffe_deep_dreams

FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04

RUN apt-get update && apt-get install -y --no-install-recommends \
            build-essential \
            pkg-config \
	        dh-autoreconf \       
            cmake \     
            unzip \
            git \
            wget \            
            nano \
            libboost-all-dev \
            libgflags-dev \
            libgoogle-glog-dev \
            libhdf5-serial-dev \
            libleveldb-dev \
            liblmdb-dev \
            libprotobuf-dev \
            libsnappy-dev \
            libavcodec-dev \
            libavformat-dev \
            libswscale-dev \
            libtbb2 \
            libtbb-dev \
            libjpeg-dev \
            libpng-dev \
            libtiff-dev \
            libjasper-dev \
            libdc1394-22-dev \
            libxine2-dev \
            zlib1g-dev \
            libvorbis-dev \
            libxvidcore-dev \
            libgstreamer0.10-dev \
            libgstreamer-plugins-base0.10-dev \
            gstreamer-tools \
            libv4l-dev \
            v4l-utils \
            libgdal-dev \
            libeigen3-dev \
            libcurl3-dev \
            x264 \
            x265 \
            protobuf-compiler \
            swig \
            rsync \
            ffmpeg \ 
            qtbase5-dev \
            libgtk-3-dev \
            libfaac-dev \
            libmp3lame-dev \
            libopencore-amrnb-dev \
            libopencore-amrwb-dev \
            libtheora-dev \
            libvorbis-dev \ 
            libxvidcore-dev \
            v4l-utils \
            python-vtk \
            liblapacke-dev \
            libopenblas-dev \    
            libatlas-base-dev \        
    dh-autoreconf && \
    apt-get clean && \ 
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN apt-get update && apt-get install -y --no-install-recommends \
            python-dev \
            python3-dev \
            python-numpy \
            python3-numpy \
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

RUN cd /opt && \
    git clone --branch master https://github.com/opencv/opencv.git opencv && \
    cd /opt/opencv && \
	git checkout de35c59ba4cf863d014e130c9116a0d5dade7c91 && \
	mkdir build && cd build && \
	cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D CUDA_FAST_MATH:BOOL="1" -D WITH_NVCUVID:BOOL="1" -D WITH_CUBLAS:BOOL="1" -D FORCE_VTK=ON -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D WITH_CUBLAS=ON -D CUDA_NVCC_FLAGS="-D_FORCE_INLINES" -D WITH_GDAL=ON -D WITH_XINE=ON -D BUILD_PERF_TESTS=OFF -DBUILD_TESTS=OFF .. && \
	make -j $(($(nproc) + 1)) && \
	make -j $(($(nproc) + 1)) install && \
	echo "/usr/local/lib" | tee -a /etc/ld.so.conf.d/opencv.conf && \
	ldconfig

RUN cd /opt && \
    git clone --depth 1 https://github.com/BVLC/caffe.git caffe && \
    cd caffe && \   
    mkdir build && \
    cd python && \
    pip2 install -r requirements.txt && \
    pip3 install -r requirements.txt && \
    cd ../build && \
    cmake -DUSE_CUDNN=ON .. && \
    make -j $(($(nproc) + 1)) && \
    cd ../scripts && \
    ./download_model_binary.py ../models/bvlc_googlenet/

RUN apt-get update && apt-get install -y --no-install-recommends \
        ipython \
        ipython-notebook && \
    pip2 install jupyter && \
    pip3 install jupyter && \
    python2 -m pip install ipykernel && \
    python2 -m ipykernel install --user && \
    python3 -m pip install ipykernel && \
    python3 -m ipykernel install --user && \    
    cd /opt && \
    git clone https://github.com/google/deepdream deep_dream

ENV CAFFE_ROOT=/opt/caffe
ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

EXPOSE 8888
WORKDIR /opt/deep_dream
CMD ["ipython2", "notebook", "--no-browser", "--ip=0.0.0.0", "--allow-root"]

