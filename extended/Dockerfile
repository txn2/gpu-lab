FROM txn2/gpu-lab-base:v2.0.1

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

# houkeeping from base containers
RUN rm /etc/apt/sources.list.d/nvidia-machine-learning.list

# deps
RUN apt update \
 && apt upgrade -y \
 && apt autoremove -y \
 && apt install -y figlet octave gnuplot nodejs

# kubernetes
RUN curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add - \
 && echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list \
 && apt update \
 && apt install -y kubectl

RUN wget https://github.com/txn2/kubefwd/releases/download/1.17.2/kubefwd_amd64.deb \
 && apt install -y ./kubefwd_amd64.deb \
 && rm ./kubefwd_amd64.deb

# minio cli
RUN curl https://dl.min.io/client/mc/release/linux-amd64/mc -o /usr/bin/mc \
 && chmod +x /usr/bin/mc

# tensorflow and pytorch are installed in the
# base container.
# add and upgrade kernels and libs
RUN pip install --upgrade --no-cache \
    autopep8 \
    awscli \
    azure-storage-blob==12.3.0 \
    bash_kernel \
    bokeh \
    boto3 \
    cloudpickle \
    elasticsearch \
    Faker \
    fastai \
    gensim \
    ipywidgets \
    jupyter-lsp \
    jupyterlab-git \
    kafka-python \
    kaleido \
    kubernetes \
    matplotlib \
    minio \
    mlflow \
    nbgitpuller \
    nbresuse \
    numpy \
    octave_kernel \
    paho-mqtt \
    pandas \
    pandasticsearch \
    plotly==4.10.0 \
    ptvsd \
    py-solc \
    pydot \
    pylantern \
    python-gitlab \
    python-language-server[all] \
    rubix \
    seaborn \
    scikit-learn \
    scipy \
    scrapy \
    statsmodels \
    web3 \
    xeus-python==0.7.1

# install R language server
RUN R -e 'install.packages("languageserver", repos = "http://cran.us.r-project.org")'

RUN python -m bash_kernel.install

RUN jupyter labextension install \
    @jupyterlab/debugger \
    @jupyterlab/mathjax3-extension \
    @jupyterlab/katex-extension \
    @jupyterlab/geojson-extension \
    @jupyterlab/git \
    @jupyterlab/fasta-extension \
    @krassowski/jupyterlab-lsp \
    jupyterlab-system-monitor \
    jupyterlab-drawio \
    jupyterlab-plotly@4.10.0 \
    plotlywidget@4.10.0

# extension management
RUN jupyter serverextension enable --py jupyterlab_git \
 && jupyter labextension disable @jupyterlab/extensionmanager-extension \
 && jupyter lab build

# Fix permissions
RUN fix-permissions $CONDA_DIR
RUN fix-permissions /home/$NB_USER

COPY page_config.json /opt/conda/share/jupyter/lab/settings/page_config.json
COPY overrides.json /opt/conda/share/jupyter/lab/settings/overrides.json
COPY .bash_profile /home/$NB_USER/.bash_profile
RUN chown $NB_UID:$NB_GID /home/$NB_USER/.bash_profile

RUN apt-get clean && rm -rf /var/lib/apt/lists/ && rm -rf /tmp/*


USER $NB_UID