FROM ubuntu:16.04
MAINTAINER Casey Dunn (casey.dunn@yale.edu)

# System configuration

# Set up access to outside world
EXPOSE 9090

# Install delendencies
RUN apt-get update && apt-get install -y \
#    wget \
    unzip \
    rsync


# Download and install SMRT Link v5.0.1 per instructions at http://www.pacb.com/support/software-downloads/

# RUN wget https://downloads.pacbcloud.com/public/software/installers/smrtlink_5.0.1.9585.zip
ADD https://downloads.pacbcloud.com/public/software/installers/smrtlink_5.0.1.9585.zip smrtlink_5.0.1.9585.zip

RUN unzip -P nX4dmaR% smrtlink_5.0.1.9585.zip

ENV SMRT_USER=root
ENV SMRT_ROOT=/opt/pacbio/smrtlink
RUN ./smrtlink_5.0.1.9585.run --rootdir $SMRT_ROOT