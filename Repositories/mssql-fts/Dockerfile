# mssql-agent-fts-ha-tools
# Maintainers: Microsoft Corporation (twright-msft on GitHub)
# GitRepo: https://github.com/Microsoft/mssql-docker

# Base OS layer: Latest Ubuntu LTS
FROM ubuntu:16.04

ENV ACCEPT_EULA=Y

#Install curl since it is needed to get repo config
# Get official Microsoft repository configuration
RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -y curl apt-transport-https && \
    curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list | tee /etc/apt/sources.list.d/mssql-server.list && \
    curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | tee /etc/apt/sources.list.d/msprod.list && \
    apt-get update

# Install SQL Server which a prerequisite for the optional packages below.
# Install optional packages.  Comment out the ones you don't need
RUN apt-get update && \
    apt-get install -y mssql-server mssql-server-ha mssql-server-fts mssql-tools 
    # apt-get install -y mssql-server-ha && \
    # apt-get install -y mssql-server-fts && \
    # apt-get install -y mssql-tools && \

# Run SQL Server process
CMD /opt/mssql/bin/sqlservr
