FROM ubuntu:16.04
MAINTAINER Casey Dunn (casey.dunn@yale.edu)

# System configuration
RUN chmod a+w /opt

# Set up access to outside world
EXPOSE 9090-9095
EXPOSE 8243

# Install delendencies
RUN apt-get update && apt-get install -y \
    curl \
    default-jre \
    net-tools \
    unzip \
    rsync

# Satisfy SMRT Link Server Environment Assumptions

## "A 64-bit Linux host with a libc version greater than 2.5"
## But the most recent vertsion of glibc is only at 2.23, maybe libc versioning is different than glibc versioning?

## "ensure that the host system locale is set to LANG=en_US.UTF-8"
RUN locale-gen "en_US.UTF-8"
RUN update-locale LANG=en_US.UTF-8


# Download and install SMRT Link v5.0.1 per instructions at http://www.pacb.com/support/software-downloads/
ADD https://downloads.pacbcloud.com/public/software/installers/smrtlink_5.0.1.9585.zip smrtlink_5.0.1.9585.zip
RUN unzip -P nX4dmaR% smrtlink_5.0.1.9585.zip

ENV SMRT_USER=smrtanalysis
RUN useradd -ms /bin/bash $SMRT_USER
USER $SMRT_USER


ENV SMRT_ROOT=/opt/pacbio/smrtlink
RUN ./smrtlink_5.0.1.9585.run --rootdir $SMRT_ROOT --batch --ignore-system-check

CMD $SMRT_ROOT/admin/bin/services-start
CMD $SMRT_ROOT/admin/bin/run-sat-services