# smrtlink dockerfile

*This is a work in progress. It might not work yet.*

This respository contains everything needed to get PacBio SMRTLink software running in a Docker container. The SMRTLink download and documentation are available from PacBio at http://www.pacb.com/support/software-downloads/ . The Dockerfile presented here builds a Docker container that has a running copy of the software. You don't need to download anything from here, just make sure you have a system that meets the minimum requirements and execute the commands below to build and run the container.

## Host system

SMRTlink needs a big computer. Much bigger than laptops and most desktops. Here are the system requirements at present:


    The recommended and minimum configurations are:
       Total Physical Memory:        64 GB (recommended), 32 GB (minimum)
       Total Number of Processors:   32    (recommended), 16    (minimum)
       Minimum open files limit   ('ulimit -n'):  8192  (required)
       Minimum user process limit ('ulimit -u'):  8192  (required)

Having less than the required limits will result in a warning, and by default installation will not complete if there are any warnings. 

### Amazon Web Services

I developed this Dockerfile on an [m4.10xlarge](https://aws.amazon.com/ec2/instance-types/) EC2 instance, which exceeds the above recommended configuration, running Amazon Linux. I made the following changes to the default configuration:

- Increase the storage to 100 GB

- Open port 9090 (for user access once it is running) and 443 (for git access during installation) 

Once the instance is running, increase the open files limit. I took the approach presented at http://aws-labs.com/increase-open-files-limit/ Paste the following into `/etc/security/limits.conf` just before the line `# End of file`: 

    *         hard    nofile      500000
    *         soft    nofile      500000
    root      hard    nofile      500000
    root      soft    nofile      500000

Reboot the amazon machine for the new settings to take effect. You can run `ulimit -Sn` to see if the limit was reset. 

Now install docker with:

    sudo yum update -y
    sudo yum install -y docker
    sudo yum install -y git
    sudo service docker start
    sudo usermod -a -G docker ec2-user

Log out and then log in again for the `usermod` changes to take effect.

Your Amazon machine is now ready to build and run the SMRTLink Docker container.

## Build and run

To build and run the docker container, execute:

    docker build --ulimit nofile=500000:500000 https://github.com/caseywdunn/docker-smrtlink.git#master:docker
