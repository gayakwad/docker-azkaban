FROM java:8

# Never prompts the user for choices on installation/configuration of packages
ENV DEBIAN_FRONTEND noninteractive
ENV TERM linux
# Work around initramfs-tools running on kernel 'upgrade': <http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=594189>
ENV INITRD No

ENV AZK_VERSION 3.40.0
ENV MYSQL_JDBC_VERSION 5.1.35

WORKDIR /root

COPY dist/azkaban-exec-server-$AZK_VERSION.tar.gz .
RUN tar -xzvf azkaban-exec-server-$AZK_VERSION.tar.gz \
    && mkdir azkaban-exec-server-$AZK_VERSION/extlib/

RUN apt-get update -yqq \
    && apt-get install -yqq \
    curl \
    && curl -sLk http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-$MYSQL_JDBC_VERSION.tar.gz| tar xz \
    && mv mysql-connector-java-$MYSQL_JDBC_VERSION/mysql-connector-java-$MYSQL_JDBC_VERSION-bin.jar azkaban-exec-server-$AZK_VERSION/extlib/ \
    && rm -rf mysql-connector-java-$MYSQL_JDBC_VERSION \
    # Work around to run container as a daemon
    && sed -i "s/&//" azkaban-exec-server-$AZK_VERSION/bin/azkaban-executor-start.sh \
    && apt-get clean \
    && rm -rf \
    /var/lib/apt/lists/* \
    /tmp/* \
    /var/tmp/* \
    /usr/share/man \
    /usr/share/doc \
    /usr/share/doc-base

COPY conf/azkaban.properties azkaban-exec-server-$AZK_VERSION/conf/azkaban.properties
COPY conf/log4j.properties azkaban-exec-server-$AZK_VERSION/conf/log4j.properties
COPY conf/global.properties azkaban-exec-server-$AZK_VERSION/conf/global.properties

# Azkaban executor port
EXPOSE 12321
# Define default workdir
WORKDIR /root/azkaban-exec-server-$AZK_VERSION

CMD ["bin/azkaban-executor-start.sh"]
