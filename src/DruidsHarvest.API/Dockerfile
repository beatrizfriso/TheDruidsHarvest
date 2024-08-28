# Use a base image with ASP.NET runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0-nanoserver-1809 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Use a base image with .NET SDK for build
FROM mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-1809 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["DruidsHarvest.API/DruidsHarvest.API.csproj", "DruidsHarvest.API/"]
RUN dotnet restore "./DruidsHarvest.API/DruidsHarvest.API.csproj"
COPY . .
WORKDIR "/src/DruidsHarvest.API"
RUN dotnet build "./DruidsHarvest.API.csproj" -c ${BUILD_CONFIGURATION} -o /app/build

# Publish the application
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./DruidsHarvest.API.csproj" -c ${BUILD_CONFIGURATION} -o /app/publish /p:UseAppHost=false

# Create the final image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DruidsHarvest.API.dll"]

# Add metadata
LABEL org.opencontainers.image.title="DruidsHarvest API" \
      org.opencontainers.image.version="1.0" \
      org.opencontainers.image.description="API for DruidsHarvest application" \
      org.opencontainers.image.authors="Beatriz Friso <beatriz_f.p@hotmail.com>"
