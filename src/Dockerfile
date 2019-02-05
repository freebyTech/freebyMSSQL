# 
#   Builds an already initialized Jenkins instance with access to certain resources
#   setup by default.
#    
#   Authors: James Eby
#
#   The Microsoft SQL Server docker build for SQL Server Unit and System Test Scenarios.
#   Contains sqlpackage and the dotnet core 2.1 SDK for dacpac installs.
#
#   Much of this was sourced from:
#   https://hub.docker.com/r/samuelmarks/mssql-server-fts-sqlpackage-linux/

FROM mcr.microsoft.com/mssql/server:2017-latest
LABEL maintainer="James@freebyTech.com"

RUN apt-get update && apt-get install -y curl apt-transport-https software-properties-common locales unzip

RUN echo DOTNET_CLI_TELEMETRY_OPTOUT=1 >> /etc/environment
RUN locale-gen en_US.UTF-8 &amp;&amp; \
  printf 'LANGUAGE=en_US.UTF-8\nLC_ALL=en_US.UTF-8\n' >> /etc/default/locale

RUN mkdir -p /opt/downloads
WORKDIR /opt/downloads

RUN curl -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O
RUN dpkg -i packages-microsoft-prod.deb 

RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"
RUN add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list)"

RUN apt-get update && ACCEPT_EULA=Y apt-get install -y dotnet-sdk-2.1 

RUN echo PATH="$PATH:/opt/mssql-tools/bin" >> ~/.bash_profile
RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc

RUN curl -Lq https://go.microsoft.com/fwlink/?linkid=2069122 -o sqlpackage-linux-x64-latest.zip
RUN ls -alh
RUN unzip sqlpackage-linux-x64-latest.zip -d /opt/sqlpackage