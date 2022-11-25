FROM ubuntu:22.04
LABEL org.opencontainers.image.title="Estimation of Human Base Kinematics using Dynamical Inverse Kinematics and Contact-Aided Lie Group Kalman Filter"
LABEL org.opencontainers.image.description="Infrastructure for reproducing experiments presented in the paper"
LABEL org.opencontainers.image.source="https://raw.githubusercontent.com/ami-iit/paper_ramadoss_2022_humanoids_human-base-estimation/main/dockerfiles/Dockerfile"
LABEL org.opencontainers.image.authors="Prashanth Ramadoss"

ARG username=user
ARG userid=1000
ARG groupid=100

SHELL ["/bin/bash", "-c"]

# Non-interactive installation mode
ENV DEBIAN_FRONTEND=noninteractive

# Set the locale
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

# Create non-root user for safety
ENV USER $username
ENV UID $userid
ENV GID $groupid
ENV HOME /home/$USER

RUN adduser --disabled-password \
    --gecos "Non-root User" \
    --uid ${UID} \
    --gid ${GID} \
    --home $HOME \
    $USER

# COPY dependency files to a /tmp/ folder and install apt packages
COPY deps/apt-packages.txt /tmp/
COPY deps/robotology-deps.yml /tmp/

RUN chown $UID:$GID /tmp/apt-packages.txt && \
    chown $UID:$GID /tmp/robotology-deps.yml && \
    apt-get update > /dev/null && \
    xargs apt-get install --no-install-recommends --yes < /tmp/apt-packages.txt > /dev/null && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p $HOME/.config && \
    chown $UID:$GID $HOME/.config

# Switch to non-root user
USER ${USER}

# Prepare conda environment to install robotology superbuild
ENV CONDA_DIR=$HOME/conda
ENV PATH=${CONDA_DIR}/bin:${PATH}
RUN  wget --no-hsts --quiet "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh" -O ~/mambaforge_installer.sh && \
    sh ~/mambaforge_installer.sh -b -p ${CONDA_DIR} && \
    rm ~/mambaforge_installer.sh && \
    mamba clean --tarballs --index-cache --packages --yes && \
    find ${CONDA_DIR} -follow -type f -name '*.a' -delete && \
    find ${CONDA_DIR} -follow -type f -name '*.pyc' -delete && \
    mamba clean --all --yes && \
    mamba create -n robsub python=3.8 && \
    echo ". ${CONDA_DIR}/etc/profile.d/conda.sh && conda activate robsub" >> ~/.bashrc

RUN mamba env update --name robsub \
                     --file /tmp/robotology-deps.yml && \
    mamba clean --tarballs --index-cache --packages --yes && \
    find ${CONDA_DIR} -follow -type f -name '*.pyc' -delete && \
    mamba clean --all --yes

# Create an IIT Workspace and clone repositories inside
ENV WSDIR  $HOME/iit_ws
RUN mkdir -p $WSDIR
WORKDIR $WSDIR

ENV PATH=$CONDA_DIR/bin:$CONDA_DIR/envs/robsub/bin:$PATH
ENV TERM xterm-256color

RUN cd $WSDIR && \
    git clone https://github.com/ami-iit/kindynfusion.git && \
    cd kindynfusion/src/ && \
    git fetch origin humanoids2022:humanoids2022 && \
    git checkout humanoids2022 && \
    mkdir -p  build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=Release \
      -DNON_INTERACTIVE_BUILD:BOOL=ON \
      -DALLOW_IDL_GENERATION=ON \
      -DCMAKE_INSTALL_PREFIX=$WSDIR/kindynfusion/install .. && \
    make install && \
    cd .. && rm -rf build

ADD dataset $WSDIR/dataset
COPY conf/tmux-humanoids.yml /home/user/.config/tmuxinator/humanoids22.yml

RUN echo "export KinDynFusion_DIR=${WSDIR}/kindynfusion/install" >> ~/.bashrc && \
    echo "export YARP_DATA_DIRS=${YARP_DATA_DIRS}:/home/user/iit_ws/kindynfusion/install/share/yarp:/home/user/conda/envs/robsub/share/yarp:/home/user/conda/envs/robsub/share/human-gazebo" >> ~/.bashrc && \
    echo "export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/home/user/iit_ws/kindynfusion/install/lib" >> ~/.bashrc && \
    yarp namespace /robot

#CMD ["tmuxinator", "start", "humanoids22"]



