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

# Instalación de Java 
RUN mkdir -p /etc/apt/keyrings \
 && wget -O /etc/apt/keyrings/adoptium.asc https://packages.adoptium.net/artifactory/api/gpg/key/public \
 && echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(lsb_release -cs) main" \
    > /etc/apt/sources.list.d/adoptium.list \
 && apt-get update \
 && apt-get install -y temurin-21-jdk \
 && rm -rf /var/lib/apt/lists/*

# --- SOLUCIÓN AL ERROR DE JAVA_HOME ---
# Buscamos la carpeta real de temurin y creamos un link simbólico estándar
RUN ln -s $(ls -d /usr/lib/jvm/temurin-21-jdk-*) /usr/lib/jvm/java-21-runtime

ENV JAVA_HOME=/usr/lib/jvm/java-21-runtime
ENV PATH=$JAVA_HOME/bin:$PATH

WORKDIR /opt

# Instalación de WSO2 APIM 
RUN wget https://github.com/wso2/product-apim/releases/download/v4.6.0/wso2am-4.6.0.zip \
 && unzip wso2am-4.6.0.zip \
 && rm wso2am-4.6.0.zip

# Asegurar permisos de ejecución 
RUN chmod +x /opt/wso2am-4.6.0/bin/*.sh

EXPOSE 9443 9763 8243 8280

WORKDIR /opt/wso2am-4.6.0/bin

CMD ["sh", "api-manager.sh"]