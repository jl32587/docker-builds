# FROM defines the base docker image. This command has to come first in the file
# The 'as' keyword lets you name the folowing stage. We use `app` for the production image
FROM --platform=linux/x86_64 ubuntu:focal as app

# ARG sets environment variables during the build stage
ARG IRMA_VER="1.0.3"

# LABEL instructions tag the image with metadata that might be important to the user
# Optional, but highly recommended
LABEL base.image="ubuntu:xenial"
LABEL dockerfile.version="2"
LABEL software="IRMA"
LABEL software.version=$IRMA_VER
LABEL description="IRMA was designed for the robust assembly, variant calling, and phasing of highly variable RNA viruses. Currently IRMA is deployed with modules for influenza, ebolavirus and coronavirus."
LABEL website="https://wonder.cdc.gov/amd/flu/irma/"
LABEL license="https://wonder.cdc.gov/amd/flu/irma/disclaimer.html"
LABEL maintainer="John Arnn"
LABEL maintainer.email="jarnn@utah.gov"

# RUN executes code during the build
# Install dependencies via apt-get or yum if using a centos or fedora base
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
 perl \
 default-jre \
 apt-transport-https \
 ca-certificates \
 gnupg \
 libcurl4-openssl-dev \
 libssl-dev \
 libxml2 \
 libxml2-dev \
 libncurses5 \
 wget \
 gzip \
 git && \
 apt-get autoclean && rm -rf /var/lib/apt/lists/*

# IRMA Needs R
RUN  apt-get update && apt-get -y upgrade && apt-get -y install r-base \
  r-base-dev

 # install IRMA
RUN wget https://wonder.cdc.gov/amd/flu/irma/flu-amd-202209.zip && \
  unzip flu-amd-202209.zip && \
  rm flu-amd-202209.zip && \
  chmod 755 -R flu-amd && \
  mkdir data



# set the environment
ENV PATH="${PATH}:/flu-amd"


# WORKDIR sets working directory
WORKDIR /data


FROM app as test

RUN cd /flu-amd/tests && \
  ./test_run.sh

RUN wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR179/072/SRR17940172/SRR17940172_1.fastq.gz && gzip -d SRR17940172_1.fastq.gz && \
    wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR179/072/SRR17940172/SRR17940172_2.fastq.gz && gzip -d SRR17940172_2.fastq.gz
    

#IRMA does not like the formatting for SRA paired reads; Expects Illumina Formatting; this line of code will fix this issue
RUN awk '{if(NR%2==1) $0=sprintf($1); print;}' SRR17940172_1.fastq > SRR17940172_1a.fastq && \
  awk '{if(NR%4==1) $0=sprintf($1" 1:N:18:NULL"); print;}' SRR17940172_1a.fastq > SRR17940172_1.fastq && \
  awk '{if(NR%2==1) $0=sprintf($1); print;}' SRR17940172_2.fastq > SRR17940172_2a.fastq && \
  awk '{if(NR%4==1) $0=sprintf($1" 2:N:18:NULL"); print;}' SRR17940172_2a.fastq > SRR17940172_2.fastq
  
RUN IRMA FLU SRR17940172_1.fastq SRR17940172_2.fastq SRR17940172
