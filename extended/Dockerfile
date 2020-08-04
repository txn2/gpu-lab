FROM txn2/gpu-lab-base:v1.0.0

LABEL maintainer="Craig Johnston <cj@imti.co>"

ARG NB_USER="jovyan"
ARG NB_UID="1000"
ARG NB_GID="100"

ENV CONDA_DIR=/opt/conda \
    SHELL=/bin/bash \
    JUPYTERHUB_USER=tester \
    PLATFORM_NAME=txn2 \
    PLATFORM_DOMAIN=txn2.com \
    NB_USER=$NB_USER \
    NB_UID=$NB_UID \
    NB_GID=$NB_GID

USER root

# deps
RUN apt-get update
RUN apt-get install -y figlet octave gnuplot

RUN pip install --upgrade jupyterlab-git
RUN jupyter serverextension enable --py jupyterlab_git
RUN jupyter labextension install @jupyterlab/git

# add and upgrade kernels and libs
RUN pip install --upgrade --no-cache \
    dask[complete] \
    jupyterlab-nvdashboard \
    autopep8 \
    bash_kernel \
    nbresuse \
    pylantern \
    ipywidgets \
    elasticsearch \
    pandasticsearch \
    kafka-python \
    kubernetes \
    nbgitpuller \
    rubix \
    python-gitlab \
    scipy \
    numpy \
    pandas \
    scikit-learn \
    matplotlib \
    tensorflow \
    torch \
    torchvision \
    fastai \
    octave_kernel \
    paho-mqtt \
    web3 \
    py-solc

RUN python -m bash_kernel.install

RUN jupyter labextension install \
    @jupyterlab/debugger \
    @jupyterlab/mathjax3-extension \
    @jupyterlab/katex-extension \
    @jupyterlab/geojson-extension \
    @jupyterlab/git \
    @jupyterlab/fasta-extension \
    jupyterlab-nvdashboard \
    jupyterlab-system-monitor \
    jupyterlab-drawio

# disable user extension management
RUN jupyter labextension disable @jupyterlab/extensionmanager-extension

# Fix permissions
RUN fix-permissions $CONDA_DIR
RUN fix-permissions /home/$NB_USER

COPY page_config.json /opt/conda/share/jupyter/lab/settings/page_config.json
COPY overrides.json /opt/conda/share/jupyter/lab/settings/overrides.json
COPY .bash_profile /home/$NB_USER/.bash_profile
RUN chown $NB_UID:$NB_GID /home/$NB_USER/.bash_profile

USER $NB_UID