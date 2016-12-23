FROM ubuntu:latest

ENV PHOENIX_VERSION=4.8.2-HBase-1.2 \
    HBASE_VERSION=1.2.4 \
    JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 \
    APACHE_MIRROR=http://apache.osuosl.org

RUN apt-get -y update \
    && apt-get -y install wget openjdk-8-jdk-headless python \
    && mkdir -p /opt/phoenix /opt/hbase \
    && wget --no-verbose -P /tmp -c -N \
        $APACHE_MIRROR/hbase/$HBASE_VERSION/hbase-$HBASE_VERSION-bin.tar.gz \
    && tar xf /tmp/hbase-$HBASE_VERSION-bin.tar.gz --strip-components=1 -C /opt/hbase \
    && wget --no-verbose -P /tmp -c -N \
        $APACHE_MIRROR/phoenix/apache-phoenix-$PHOENIX_VERSION/bin/apache-phoenix-$PHOENIX_VERSION-bin.tar.gz \
    && tar xf /tmp/apache-phoenix-$PHOENIX_VERSION-bin.tar.gz --strip-components=1 -C /opt/phoenix \
    && echo 'JAVA_HOME=$JAVA_HOME' | cat - /opt/hbase/conf/hbase-env.sh > temp && mv temp /opt/hbase/conf/hbase-env.sh \
    && ln -s /opt/phoenix/phoenix-$PHOENIX_VERSION-server.jar /opt/hbase/lib/phoenix-$PHOENIX_VERSION-server.jar

CMD /opt/hbase/bin/start-hbase.sh \
    && (/opt/phoenix/bin/queryserver.py start &) \
    && tail -f /tmp/phoenix/root-queryserver.log
