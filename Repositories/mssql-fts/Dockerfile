# mssql-agent-fts-ha-tools
# Maintainers: Microsoft Corporation (twright-msft on GitHub)
# GitRepo: https://github.com/Microsoft/mssql-docker

# Base OS layer: Latest Ubuntu LTS
FROM ubuntu:16.04

#Install curl since it is needed to get repo config
# Get official Microsoft repository configuration
RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -y curl && \
    apt-get install apt-transport-https && \
    curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list | tee /etc/apt/sources.list.d/mssql-server.list && \
    apt-get update

# Install SQL Server which a prerequisite for the optional packages below.
# Install optional packages.  Comment out the ones you don't need
RUN apt-get update && \
    apt-get install -y mssql-server && \
    apt-get install -y mssql-server-ha && \
    apt-get install -y mssql-server-fts

# Google Cloud APT key
RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

# Install Google Cloud SDK
RUN export CLOUD_SDK_REPO="cloud-sdk-xenial" && \
    echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
    apt-get update && apt-get install -y google-cloud-sdk kubectl

# GCSFuse used for backups to Google Cloud Storage
# https://github.com/GoogleCloudPlatform/gcsfuse/blob/master/docs/installing.md
RUN export GCSFUSE_REPO="gcsfuse-xenial" && \
    echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | tee /etc/apt/sources.list.d/gcsfuse.list && \
    apt-get update && apt-get install -y gcsfuse

# A directory to hold gcsfuse mount for backups
RUN mkdir /mnt/sqlbackup

# Set volume mount points
VOLUME /var/opt/mssql/data
VOLUME /var/backups

# Run SQL Server process
CMD /opt/mssql/bin/sqlservr