FROM java:8

ENV DEBIAN_FRONTEND noninteractive
ENV TERM linux
ENV INITRD No

ENV AZK_VERSION 3.40.0
ENV MYSQL_JDBC_VERSION 5.1.35

WORKDIR /root

COPY dist/azkaban-web-server-3.40.0.tar.gz /root/
RUN tar -xzvf azkaban-web-server-$AZK_VERSION.tar.gz \
    && mkdir azkaban-web-server-$AZK_VERSION/extlib/

COPY dist/azkaban-db-$AZK_VERSION.tar.gz .
RUN tar -xzvf azkaban-db-$AZK_VERSION.tar.gz

RUN apt-get update -yqq \
    && apt-get install -yqq \
    curl \
    netcat \
    mysql-client \
    && curl -sLk http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-$MYSQL_JDBC_VERSION.tar.gz| tar xz \
    && mv mysql-connector-java-$MYSQL_JDBC_VERSION/mysql-connector-java-$MYSQL_JDBC_VERSION-bin.jar azkaban-web-server-$AZK_VERSION/extlib/ \
    && rm -rf mysql-connector-java-$MYSQL_JDBC_VERSION \
    && mkdir azkaban-web-server-$AZK_VERSION/logs/ \
    # Work around to run container as a daemon
    && sed -i "s/&//" azkaban-web-server-$AZK_VERSION/bin/azkaban-web-start.sh \
    && apt-get clean \
    && rm -rf \
    /var/lib/apt/lists/* \
    /tmp/* \
    /var/tmp/* \
    /usr/share/man \
    /usr/share/doc \
    /usr/share/doc-base

ADD keystore azkaban-web-server-$AZK_VERSION/keystore
ADD conf/azkaban.properties azkaban-web-server-$AZK_VERSION/conf/azkaban.properties
ADD conf/log4j.properties azkaban-web-server-$AZK_VERSION/conf/log4j.properties
ADD conf/global.properties azkaban-web-server-$AZK_VERSION/conf/global.properties
ADD conf/azkaban-users.xml azkaban-web-server-$AZK_VERSION/conf/azkaban-users.xml

# Azkaban web server port
EXPOSE 8443

# Define default workdir
WORKDIR /root/azkaban-web-server-$AZK_VERSION
ADD script/run.sh run.sh
RUN chmod +x run.sh

CMD ["./run.sh"]
