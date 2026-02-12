FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Dependencias base
RUN apt-get update && apt-get install -y \
    wget \
    unzip \
    ca-certificates \
    gnupg \
    lsb-release \
    netcat \
 && rm -rf /var/lib/apt/lists/*

# Temurin OpenJDK 21
RUN mkdir -p /etc/apt/keyrings \
 && wget -O /etc/apt/keyrings/adoptium.asc https://packages.adoptium.net/artifactory/api/gpg/key/public \
 && echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(lsb_release -cs) main" \
    > /etc/apt/sources.list.d/adoptium.list \
 && apt-get update \
 && apt-get install -y temurin-21-jdk \
 && rm -rf /var/lib/apt/lists/*

ENV JAVA_HOME=/usr/lib/jvm/temurin-21-jdk-arm64
ENV PATH=$JAVA_HOME/bin:$PATH

# Instalar WSO2 APIM
WORKDIR /opt

RUN wget https://github.com/wso2/product-apim/releases/download/v4.6.0/wso2am-4.6.0.zip \
 && unzip wso2am-4.6.0.zip \
 && rm wso2am-4.6.0.zip

# Puertos
EXPOSE 9443 9763 8243 8280

WORKDIR /opt/wso2am-4.6.0/bin

CMD ["sh", "api-manager.sh"]
