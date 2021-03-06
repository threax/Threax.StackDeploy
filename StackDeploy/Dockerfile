﻿FROM mcr.microsoft.com/dotnet/core/runtime:2.2 AS base
# Install docker
WORKDIR /docker
RUN apt-get update &&\
    apt-get install wget -y &&\
    apt-get install apt-transport-https -y &&\
    wget -q https://download.docker.com/linux/static/stable/x86_64/docker-18.09.1.tgz &&\
    tar xzvf docker-18.09.1.tgz &&\
    mv docker/* /usr/bin/ &&\
    rm docker-18.09.1.tgz &&\
    apt-get remove --auto-remove apt-transport-https -y &&\
    apt-get remove --auto-remove wget -y &&\
    apt-get autoremove &&\
    apt-get clean

WORKDIR /app

FROM threax/build AS build
WORKDIR /src
COPY . .
WORKDIR /src
RUN dotnet restore StackDeploy.csproj
RUN dotnet build StackDeploy.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish StackDeploy.csproj -c Release -o /app

FROM base AS final
ENV DOTNET_CLI_TELEMETRY_OPTOUT=1
WORKDIR /app
COPY --from=publish /app .
WORKDIR /data
ENTRYPOINT ["dotnet", "/app/StackDeploy.dll"]