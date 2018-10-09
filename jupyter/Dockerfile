#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

FROM predictionio/pio:latest

ENV DEBIAN_FRONTEND noninteractive

RUN apt update && apt upgrade -y && apt install -y locales && rm -rf /var/lib/apt/lists/*
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

ENV NB_USER jovyan
ENV NB_UID 1000
ENV CONDA_DIR=/opt/conda
ENV PATH $CONDA_DIR/bin:$PATH
ENV PIP_DEFAULT_TIMEOUT 180

RUN apt-get update && apt install -y build-essential curl git gcc make openssl libssl-dev libbz2-dev \
    libreadline-dev libsqlite3-dev cmake libxml2-dev wget bzip2 sudo vim unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN wget --quiet https://github.com/krallin/tini/releases/download/v0.18.0/tini && \
    echo "12d20136605531b09a2c2dac02ccee85e1b874eb322ef6baf7561cd93f93c855 *tini" | sha256sum -c - && \
    mv tini /usr/local/bin/tini && \
    chmod +x /usr/local/bin/tini

RUN wget -q https://repo.continuum.io/miniconda/Miniconda3-4.5.11-Linux-x86_64.sh -O /tmp/miniconda.sh \
    && echo 'e1045ee415162f944b6aebfe560b8fee */tmp/miniconda.sh' | md5sum -c - \
    && bash /tmp/miniconda.sh -f -b -p /opt/conda \
    && /opt/conda/bin/conda install --yes -c conda-forge \
    python=3.6 sqlalchemy tornado jinja2 traitlets requests pip nodejs configurable-http-proxy ncurses \
    && /opt/conda/bin/pip install --upgrade pip \
    && /opt/conda/bin/conda clean --all \
    && rm /tmp/miniconda.sh

ADD requirements.txt /tmp/requirements.txt
RUN python3 -m pip install notebook \
    && python3 -m pip install jupyter \
    && python3 -m pip install jupyterhub \
    && python3 -m pip install -r /tmp/requirements.txt \
    && rm -rf /root/.cache/pip/*

RUN jupyter contrib nbextension install --system

RUN useradd -G sudo -m -s /bin/bash -u $NB_UID $NB_USER \
    && echo "$NB_USER:$NB_USER" | chpasswd \
    && chown -R $NB_USER /home/$NB_USER

WORKDIR /opt/jupyterhub/
ADD jupyterhub_config.py /opt/jupyterhub/jupyterhub_config.py
ADD start-jupyterhub.sh /usr/local/bin/start-jupyterhub.sh
RUN chmod +x /usr/local/bin/*.sh

EXPOSE 8888
ENTRYPOINT ["tini", "--"]
CMD ["start-jupyterhub.sh"]

