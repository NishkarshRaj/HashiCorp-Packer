FROM ubuntu

# Packer Installation
RUN apt-get -y upgrade
RUN apt-get -y update
RUN apt install -y packer
RUN packer --version

# Packer Build AWS EC2 Instance
ADD example.json /example.json
RUN packer validate /example.json
