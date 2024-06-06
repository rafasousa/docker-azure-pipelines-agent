FROM ubuntu:24.04

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
        libicu60 \
        libunwind8 \
        netcat \
        libssl1.0 \
        wget \
        zip \
        unzip \
&& rm -rf /var/lib/apt/lists/*

# azure pipelines dependencies
# from: https://github.com/microsoft/azure-pipelines-agent/blob/25aa1872da9199dd3c7dc1721864887718ab27b4/src/Misc/layoutbin/installdependencies.sh#L107
RUN apt-get install -y libssl1.1 || apt-get install -y libssl1.0.2 || apt-get install -y libssl1.0.0 && \
    apt-get install -y libicu67 || apt-get install -y libicu66 || apt-get install -y libicu63 || apt-get install -y libicu60 || apt-get install -y libicu57 || apt-get install -y libicu55 || apt-get install -y libicu52

# other dependencies:
RUN apt-get install -y iproute2 libuuid1 libunwind8 gdebi-core build-essential libssl-dev

# tools
RUN apt-get install -y git wget curl zip unzip vim sudo iputils-ping python2 python3 python3-pip jq

# nvm/node:
RUN curl -fSsL https://deb.nodesource.com/setup_20.x | bash
RUN apt-get install -y nodejs

RUN curl -fSso- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
RUN [ "/bin/bash", "-c", "source $HOME/.nvm/nvm.sh && nvm i 14 && nvm i 16 && nvm i 18 && nvm i 20" ]
RUN [ "/bin/bash", "-c", "source $HOME/.nvm/nvm.sh && nvm alias default 20" ]

# terraform:
RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add - \
  apt-add-repository "deb [arch=$(dpkg --print-architecture)] https://apt.releases.hashicorp.com $(lsb_release -cs) main" \
  apt update \
  apt install terraform

RUN curl -LsS https://aka.ms/InstallAzureCLIDeb | bash \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /azp

COPY ./start.sh .
RUN chmod +x start.sh

CMD ["./start.sh"]