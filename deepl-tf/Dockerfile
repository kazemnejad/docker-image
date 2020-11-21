FROM continuumio/miniconda3 as base

# Needed for string substitution
SHELL ["/bin/bash", "-c"]
RUN export LANG=C.UTF-8 LC_ALL=C.UTF-8

RUN conda update -n base -c defaults conda
RUN conda install python=3.8
RUN echo "ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true" | debconf-set-selections
RUN apt-get update && apt-get install -y --no-install-recommends --fix-missing \
        curl \
        software-properties-common \
        unzip \
        apt-transport-https ca-certificates gnupg lsb-release gnupg \
        make bash-completion tree vim less tmux nmap nano wget \
        build-essential libfreetype6-dev \
        pkg-config

RUN apt-add-repository multiverse \
        && apt-get update \
        && apt-get install -y --no-install-recommends --fix-missing \
            fontconfig ttf-mscorefonts-installer \
        && update-ms-fonts && fc-cache -f -v

# Install AWS command
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
        && unzip -q awscliv2.zip \
        && chmod a+x ./aws/install \
        && ./aws/install \
        && rm awscliv2.zip \
        && rm -rf ./aws
        
# Install gcloud
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg  add - && apt-get update -y && apt-get install google-cloud-sdk -y

# Install Azure CLI
RUN curl -sL https://packages.microsoft.com/keys/microsoft.asc |
        gpg --dearmor |
        tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null
RUN echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $(lsb_release -cs) main" |
        tee /etc/apt/sources.list.d/azure-cli.list
RUN apt-get update -y && apt-get install -y azure-cli

# CUDA libraries
RUN conda install cudatoolkit=10.1 cudnn=7.6 cupti=10.1 mpi4py=3.0 nccl=2.4 -c conda-forge

RUN pip install --no-cache-dir \
        tensorflow-gpu==2.3.1 \
        tensorflow-datasets==4.0.1 \
        tensorflow-text==2.3.0 \
        tf-models-official==2.3.0 \
        fire \
        pyhocon \
        Pillow \
        h5py \
        keras_preprocessing \
        matplotlib \
        mock \
        scipy \
        sklearn \
        pandas \
        future \
        portpicker \
        enum34 \
        numpy \
        nltk==3.5 \
        jupyter \
        matplotlib \
        codalab \
        s4cmd \
        seaborn

RUN python -m nltk.downloader all

RUN pip install --no-cache-dir jupyter_http_over_ws ipykernel==5.1.1 nbformat==4.4.0

RUN apt-get clean \
        && apt-get autoremove -y \
        && rm -rf /var/lib/apt/lists/*
