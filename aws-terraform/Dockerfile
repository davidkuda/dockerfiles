FROM --platform=arm ubuntu

# Avoid command line prompts
ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install sudo
RUN apt-get update && apt-get install -y sudo

# Create the user "terraform",
# add terraform to sudoers
# and setup sudo without password
RUN adduser --disabled-password --gecos '' terraform \
    && adduser terraform sudo \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER terraform

WORKDIR /home/terraform

RUN sudo apt update && \
    sudo apt install -y \
        # Why do you need gcc?
        gcc \
        # you need git to clone the kaas repository
        git \
        # You need curl to download terraform and aws cli v2
        curl \
        # You need unzip to unzip binaries (terraform and aws cli v2)
        unzip \
    && sudo rm -rf /var/lib/apt/lists/*

# Install Terraform 1.0.0
RUN mkdir tmp \
    && curl "https://releases.hashicorp.com/terraform/1.0.0/terraform_1.0.0_linux_arm64.zip" -o "tmp/terraform.zip" \
    && unzip "tmp/terraform.zip" -d tmp \
    && sudo mv tmp/terraform /usr/bin/ \
    && rm -rf tmp

# Install aws CLI 2
RUN mkdir -p tmp && \
    curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "tmp/awscliv2.zip" && \
    unzip tmp/awscliv2.zip -d tmp && \
    sudo ./tmp/aws/install && \
    rm -r tmp

# Set aliases
RUN echo "alias t=terraform" >> /home/terraform/.bashrc \
    && echo "alias la='ls -lisah'" >> /home/terraform/.bashrc 

CMD ["bash"]