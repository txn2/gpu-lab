FROM txn2/gpu-lab-ex:v2.0.0

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

RUN jupyter labextension enable @jupyterlab/extensionmanager-extension
RUN jupyter lab build

# Fix permissions
RUN fix-permissions $CONDA_DIR
RUN fix-permissions /home/$NB_USER

USER $NB_UID