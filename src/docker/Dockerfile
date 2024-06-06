FROM ubuntu:20.04

# To make it easier for build and release pipelines to run apt-get,
# configure apt to not require confirmation (assume the -y argument by default)
ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

RUN apt-get update \
&& apt-get install -y --no-install-recommends \
        software-properties-common \
        apt-transport-https \
        ca-certificates \
        curl \
        jq \
        git \
        iputils-ping \
        libcurl4 \
        libicu66 \
        libunwind8 \
        netcat \
        libssl1.0 \
        libuuid1 \ 
        gdebi-core \
        build-essential \
        libssl-dev \
        gpg \
        wget \
        zip \
        unzip \
&& rm -rf /var/lib/apt/lists/*

# nvm/node:
RUN curl -fSsL https://deb.nodesource.com/setup_20.x | bash
RUN apt install nodejs

RUN curl -fSso- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
RUN [ "/bin/bash", "-c", "source $HOME/.nvm/nvm.sh && nvm i 18 && nvm i 20" ]
RUN [ "/bin/bash", "-c", "source $HOME/.nvm/nvm.sh && nvm alias default 20" ]

# terraform:
# RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
# RUN gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint
# # The fingerprint must match 798A EC65 4E5C 1542 8C8E 42EE AA16 FCBC A621 E701, which can also be verified at https://www.hashicorp.com/security under "Linux Package Checksum Verification".
# RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list
# RUN apt update

RUN curl -LsS https://aka.ms/InstallAzureCLIDeb | bash \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /azp

COPY ./start.sh .
RUN chmod +x start.sh

CMD ["./start.sh"]