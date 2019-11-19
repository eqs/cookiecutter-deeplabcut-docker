FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu16.04
MAINTAINER Satoshi Murashige

RUN apt-get -y update --fix-missing && apt-get -y upgrade
RUN apt-get install -y sudo wget bzip2 git vim cmake xserver-xorg-dev libgl1-mesa-dev unzip gcc g++ && \
    apt-get install -y x11-apps

# settings
ARG user_name=ubuntu
ARG user_id=1000
ARG group_name=ubuntu
ARG group_id=1000

# create user
RUN groupadd -g ${group_id} ${group_name}
RUN useradd -u ${user_id} -g ${group_id} -d /home/${user_name} \
    --create-home --shell /bin/bash ${user_name}
RUN echo "${user_name} ALL=(ALL) NOPASSWD:ALL \n" >> /etc/sudoers
RUN echo "Defaults env_keep += \"PATH\" \n" >> /etc/sudoers
RUN chown -R ${user_name}:${group_name} /home/${user_name}

# user settings
ENV HOME /home/${user_name}
ENV LANG en_US.UTF-8

USER ${user_name}

# Intall Anaconda
RUN sudo bash -c "echo 'export PATH=/home/${user_name}/anaconda3/bin:$PATH' N> /etc/profile.d/anaconda.sh" && \
    wget --quiet https://repo.continuum.io/archive/Anaconda3-2019.03-Linux-x86_64.sh -O $HOME/anaconda.sh && \
    /bin/bash $HOME/anaconda.sh -b -p $HOME/anaconda3 && \
    rm $HOME/anaconda.sh

ENV PATH $HOME/anaconda3/bin:$PATH

ENV LD_LIBRARY_PATH /usr/local/cuda-10.0/lib64:/usr/local/cuda-10.0/extras/CUPTI/lib64:$LD_LIBRARY_PATH

# ------------------------------------------------------------

WORKDIR /work

RUN sudo apt-get install -y protobuf-compiler
RUN sudo apt-get install -y ffmpeg
RUN sudo apt-get install -y pkg-config

RUN echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib' >> /home/ubuntu/.bashrc

# ------------------------------------------------------------
# Install Python packages
# ------------------------------------------------------------

RUN pip install opencv-python==3.4.5.20
RUN pip install matplotlib==3.0.3
RUN pip install tables==3.4.3
RUN pip install tensorflow-gpu==1.13.1
RUN pip install tensorboard
RUN pip install imgaug
RUN pip install https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-16.04/wxPython-4.0.3-cp37-cp37m-linux_x86_64.whl
RUN pip install deeplabcut

ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8

WORKDIR /work
CMD ["/bin/bash"]

