FROM ubuntu

# Packer Installation
RUN apt-get -y upgrade
RUN apt-get -y update
RUN apt install -y packer
RUN packer --version

# Set AWS IAM credentials as environment variables
RUN export AWS_ACCESS_KEY_ID=AKIAYNZB56UH67JQO5XJ
RUN export AWS_SECRET_ACCESS_KEY=fwQqvr7Fad60aSG/ekKjeNi5nYEC5RXUNEsmUYJl

# Packer Build AWS EC2 Instance
ADD example.json /example.json
RUN packer validate /example.json
RUN packer build example.json
