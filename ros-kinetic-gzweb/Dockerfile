FROM ubuntu:xenial

RUN apt-get clean && apt-get update && apt-get install -y locales
# setup environment
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8

# setup keys
RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 421C365BD9FF1F717815A3895523BAEEB01FA116

# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu xenial main" > /etc/apt/sources.list.d/ros-latest.list

# install bootstrap tools
RUN apt-get update && apt-get install --no-install-recommends -y \
    wget \
    python-rosdep \
    python-rosinstall \
    python-vcstools \
    python-rosinstall-generator \
    python-wstool \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# bootstrap rosdep
RUN rosdep init &&\
    rosdep update

# install ros packages
ENV ROS_DISTRO kinetic

# from kinetic-desktop-xenial
RUN apt-get update && apt-get install -y \
    ros-kinetic-desktop-full=1.3.1-0* && \
    rm -rf /var/lib/apt/lists/*

#echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
#source ~/.bashrc
# RUN source "/opt/ros/$ROS_DISTRO/setup.bash"
RUN /bin/bash -c "source /usr/share/gazebo/setup.sh" >> ~/.bashrc

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys D2486D2DD83DB69272AFE98867170598AF249743
RUN echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable xenial main" > /etc/apt/sources.list.d/gazebo-stable.list && \
    apt-get update && apt-get install -q -y \
        gazebo7 \
        libgazebo7-dev \
        imagemagick \
        libjansson-dev \
        nodejs \
        nodejs-legacy \
        npm \
        psmisc \
        xvfb \
        locate \ 
        libignition-math2-dev &&\        
    rm -rf /var/lib/apt/lists/*

# setup environment
EXPOSE 8080
EXPOSE 7681
EXPOSE 11345

RUN useradd -ms /bin/bash user

# clone gzweb
ENV GZWEB_WS /home/user/gzweb
RUN hg clone https://bitbucket.org/osrf/gzweb $GZWEB_WS && \
    /bin/bash -c "source /usr/share/gazebo/setup.sh"
WORKDIR $GZWEB_WS

# build gzweb
RUN hg up default  &&\
    xvfb-run -s "-screen 0 1280x1024x24" ./deploy.sh -m -t

USER user

#CMD gzserver --verbose & npm start