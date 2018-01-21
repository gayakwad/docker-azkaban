FROM java:8

ENV AZK_VERSION 3.40.0

COPY dist/azkaban-solo-server-$AZK_VERSION.tar.gz .

RUN tar -xvzf azkaban-solo-server-$AZK_VERSION.tar.gz

WORKDIR /azkaban-solo-server-$AZK_VERSION

EXPOSE 8081

CMD bin/azkaban-solo-start.sh; tail -f /dev/null

