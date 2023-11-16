FROM ubuntu:latest

# Set environment variables for Java and Cassandra
ENV JAVA_HOME /opt/java
ENV CASSANDRA_VERSION 5.0-alpha2
ENV CASSANDRA_HOME /opt/cassandra
ENV CASSANDRA_CONFIG /etc/cassandra
ENV PATH $JAVA_HOME/bin:$CASSANDRA_HOME/bin:$PATH

# Install dependencies
RUN apt-get update && apt-get install -y wget tar

# Download and install Adoptium JDK 21
RUN wget -O jdk.tar.gz https://api.adoptium.net/v3/binary/latest/21/ea/linux/x64/jdk/hotspot/normal/adoptium \
    && mkdir -p "$JAVA_HOME" \
    && tar --extract --file jdk.tar.gz --directory "$JAVA_HOME" --strip-components 1 \
    && rm jdk.tar.gz

# Add your install script for Cassandra and execute it
COPY install.sh /
RUN ./install.sh

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
