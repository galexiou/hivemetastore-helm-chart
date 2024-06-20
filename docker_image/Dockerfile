FROM openjdk:11-slim

ARG HADOOP_VERSION=3.3.1
ARG HIVE_VERSION=4.0.0


RUN mkdir /opt/hadoop
RUN mkdir /opt/hive-metastore

RUN apt-get update && apt-get install -y curl --no-install-recommends \
	&& rm -rf /var/lib/apt/lists/*

# Download and extract the Hadoop binary package.
RUN curl https://archive.apache.org/dist/hadoop/core/hadoop-$HADOOP_VERSION/hadoop-$HADOOP_VERSION.tar.gz \
	| tar xvz -C /opt/  \
	&& mv -v /opt/hadoop-$HADOOP_VERSION/* /opt/hadoop \
	&& rm -r /opt/hadoop/share/doc

# Add S3a jars to the classpath using this hack.
RUN ln -s /opt/hadoop/share/hadoop/tools/lib/hadoop-aws* /opt/hadoop/share/hadoop/common/lib/ && \
    ln -s /opt/hadoop/share/hadoop/tools/lib/aws-java-sdk* /opt/hadoop/share/hadoop/common/lib/


# Add Azure jars to the classpath using this hack.
RUN ln -s /opt/hadoop/share/hadoop/tools/lib/azure-data* /opt/hadoop/share/hadoop/common/lib/ && \
    ln -s /opt/hadoop/share/hadoop/tools/lib/hadoop-azure* /opt/hadoop/share/hadoop/common/lib/ && \
		ln -s /opt/hadoop/share/hadoop/tools/lib/azure-storage* /opt/hadoop/share/hadoop/common/lib/ && \
		ln -s /opt/hadoop/share/hadoop/tools/lib/azure-keyvault* /opt/hadoop/share/hadoop/common/lib/

# Set necessary environment variables.
ENV HADOOP_HOME="/opt/hadoop"
ENV PATH="/opt/spark/bin:/opt/hadoop/bin:${PATH}"

# Download and install the standalone metastore binary.
RUN curl https://archive.apache.org/dist/hive/hive-standalone-metastore-$HIVE_VERSION/hive-standalone-metastore-$HIVE_VERSION-bin.tar.gz \
	| tar xvz -C /opt/ \
	&& mv -v /opt/apache-hive-metastore-$HIVE_VERSION-bin/* /opt/hive-metastore

# Download and install the mysql connector.
RUN curl -L https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-8.0.28.tar.gz \
	| tar xvz -C /opt/ \
	&& ln -s /opt/mysql-connector-java-8.0.28/mysql-connector-java-8.0.28.jar /opt/hadoop/share/hadoop/common/lib/ \
	&& ln -s /opt/mysql-connector-java-8.0.28/mysql-connector-java-8.0.28.jar /opt/hive-metastore/lib/
