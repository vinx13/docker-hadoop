FROM openjdk:8

MAINTAINER WuweiLin <wuwei.lin@outlook.com>

WORKDIR /tmp

RUN apt-get update && apt-get install -y openssh-server wget vim

RUN wget http://mirrors.hust.edu.cn/apache/hadoop/common/hadoop-2.9.0/hadoop-2.9.0.tar.gz

RUN tar -xzf hadoop-2.9.0.tar.gz && \
    mv hadoop-2.9.0 /usr/local/hadoop && \
    rm hadoop-2.9.0.tar.gz

RUN ssh-keygen -t rsa -f ~/.ssh/id_rsa -P '' && \
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

ENV HADOOP_HOME=/usr/local/hadoop 
ENV HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
ENV PATH="/usr/local/hadoop/bin:${PATH}"
ENV JAVA_HOME=/docker-java-home

RUN mkdir -p /root/.ssh
COPY ssh_config /root/.ssh/config
COPY hadoop-env.sh $HADOOP_CONF_DIR/hadoop-env.sh
COPY core-site.xml $HADOOP_CONF_DIR/core-site.xml
COPY hdfs-site.xml $HADOOP_CONF_DIR/hdfs-site.xml
COPY yarn-site.xml $HADOOP_CONF_DIR/yarn-site.xml
COPY mapred-site.xml $HADOOP_CONF_DIR/mapred-site.xml
RUN mkdir /var/run/sshd
RUN echo "hadoop --config /usr/local/hadoop/etc/hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.0.jar bbp 1 10000 100 /bbp0"> example
RUN chmod 755 example
EXPOSE 22

