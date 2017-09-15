# smrtlink dockerfile

A Dockerfile for building a docker image to run PabBio SMRTLink software.

*This is a work in progress. It doesn't work yet*

## Host system

smrtlink needs a big computer.

### Amazon Web Services

m4.10xlarge

100 GB Storage

Open port 9090 and 443

Increase the limit on number of open files. From http://aws-labs.com/increase-open-files-limit/ , paste the following into `/etc/security/limits.conf` just before the line `# End of file`: 

    *         hard    nofile      500000
    *         soft    nofile      500000
    root      hard    nofile      500000
    root      soft    nofile      500000

Reboot the amazon machine. You can run `ulimit -Sn` to see if the limit was reset. Now install docker with:

    sudo yum update -y
    sudo yum install -y docker
    sudo yum install -y git
    sudo service docker start
    sudo usermod -a -G docker ec2-user

Log out and then log in again.

## Build

Execute:

    docker build --ulimit nofile=500000:500000 https://github.com/caseywdunn/docker-smrtlink.git#master:docker
