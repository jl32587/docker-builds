FROM ubuntu:xenial

# ARG variables only persist during build time
# had to include the v for some of these due to GitHub tags. 
# USHER ver is in the conda version style. 
# thankfully pangolearn github tag is simply a date
ARG PANGOLIN_VER="v3.1.7"
ARG PANGOLEARN_VER="2021-07-09"
ARG SCORPIO_VER="v0.3.6" 
ARG CONSTELLATIONS_VER="v0.0.11"
ARG USHER_VER="0.3.2-0"
ARG PANGO_DESIGNATION_VER="v1.2.36"

LABEL base.image="ubuntu:xenial"
LABEL dockerfile.version="1"
LABEL software="pangolin"
LABEL software.version=${PANGOLIN_VER}
LABEL description="Conda environment for Pangolin. Pangolin: Software package for assigning SARS-CoV-2 genome sequences to global lineages."
LABEL website="https://github.com/cov-lineages/pangolin"
LABEL license="GNU General Public License v3.0"
LABEL license.url="https://github.com/cov-lineages/pangolin/blob/master/LICENSE.txt"
LABEL maintainer="Erin Young"
LABEL maintainer.email="eriny@utah.gov"
LABEL maintainer2="Anders Goncalves da Silva"
LABEL maintainer2.email="andersgs@gmail.com"
LABEL maintainer3="Curtis Kapsak"
LABEL maintainer3.email="kapsakcj@gmail.com"

# install needed software for conda install; cleanup apt garbage
RUN apt-get update && apt-get install -y \
 wget \
 git \
 build-essential && \
 apt-get autoclean && rm -rf /var/lib/apt/lists/* 

# get miniconda and the pangolin repo
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
 bash ./Miniconda3-latest-Linux-x86_64.sh -p /miniconda -b  && \
 rm Miniconda3-latest-Linux-x86_64.sh && \
 wget "https://github.com/cov-lineages/pangolin/archive/${PANGOLIN_VER}.tar.gz" && \
 tar -xf ${PANGOLIN_VER}.tar.gz && \
 rm ${PANGOLIN_VER}.tar.gz && \
 mv -v pangolin-* pangolin

# set the environment
ENV PATH="/miniconda/bin:$PATH" \
 LC_ALL=C

# modify environment.yml to pin specific versions during install
# create the conda environment using modified environment.yml and set as default
RUN sed -i "s|usher.*|usher=${USHER_VER}|" /pangolin/environment.yml && \
 sed -i "s|pangoLEARN.git|pangoLEARN.git@${PANGOLEARN_VER}|" /pangolin/environment.yml && \
 sed -i "s|scorpio.git|scorpio.git@${SCORPIO_VER}|" /pangolin/environment.yml && \
 sed -i "s|constellations.git|constellations.git@${CONSTELLATIONS_VER}|" /pangolin/environment.yml && \
 sed -i "s|pango-designation.git|pango-designation.git@${PANGO_DESIGNATION_VER}|" /pangolin/environment.yml && \
 conda env create -f /pangolin/environment.yml && \
 echo "source activate pangolin" > /etc/bash.bashrc
ENV PATH /miniconda/envs/pangolin/bin:$PATH

# took this out from below step: python setup.py install (seems like the install instructions changed?)
# replaced with pip install .
RUN cd pangolin && \
 pip install . && \
 conda clean -a -y && \
 mkdir /data && \
 pangolin -v && pangolin -pv && pangolin -dv

WORKDIR /data
