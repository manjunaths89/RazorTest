
#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.




FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base

RUN apt-get install tree

RUN printf "----- DIR : INITIAL ----"
RUN tree -f

WORKDIR /app

RUN printf "----- DIR : AFTER SETTING TO /app ----"
RUN tree -f

EXPOSE 80
EXPOSE 443
RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs

RUN printf "----- DIR : BEFORE SETTING TO /src ----"
RUN tree -f

WORKDIR /src

RUN printf "----- DIR : AFTER SETTING TO /src ----"
RUN tree -f

COPY ["RazorTest.csproj", "RazorTest/"]
RUN dotnet restore "RazorTest/RazorTest.csproj"

RUN printf "----- DIR : AFTER COPYING .csproj file ----"
RUN tree -f

COPY . .

RUN printf "----- DIR : AFTER COPYING ALL TO /src ----"
RUN tree -f

WORKDIR "/src/RazorTest"

RUN printf "----- DIR : AFTER CHANGING DIR TO /src/RazorTest ----"
RUN tree -f

RUN dotnet build "RazorTest.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "RazorTest.csproj" -c Release -o /app/publish

FROM base AS final

RUN printf "----- DIR : BEFORE CHANGING DIR TO /app ----"
RUN tree -f

WORKDIR /app

RUN printf "----- DIR : AFTER CHANGING DIR TO /app ----"
RUN tree -f

COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "RazorTest.dll"]
