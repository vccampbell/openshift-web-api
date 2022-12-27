#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8081
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["openshift-web-api/openshift-web-api.csproj", "openshift-web-api/"]
RUN dotnet restore "openshift-web-api/openshift-web-api.csproj"
COPY . .
WORKDIR "/src/openshift-web-api"
RUN dotnet build "openshift-web-api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "openshift-web-api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
ENV ASPNETCORE_URLS=http://+:8081
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "openshift-web-api.dll"]