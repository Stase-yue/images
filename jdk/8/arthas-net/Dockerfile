FROM openjdk:8u342-jdk
COPY arthas-bin /opt/arthas
ENV ARTHAS_HOME=/opt/arthas
ENV PATH=$PATH:$ARTHAS_HOME
RUN apt-get update && \
    apt-get install -y wget unzip && \
    rm -rf /var/lib/apt/lists/* &&\
	echo '#!/bin/bash\ncd $ARTHAS_HOME && ./as.sh $@' > /usr/local/bin/arthas && \
    chmod +x /usr/local/bin/arthas &&\
	/bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" > /etc/timezone
