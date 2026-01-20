# ETAPA 1: Build
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /app

# Copia solo el csproj para restaurar (aprovecha la caché)
COPY *.csproj ./
RUN dotnet restore

# Copia el resto del código y publica
COPY . ./
RUN dotnet publish -c Release -o out /p:UseAppHost=false

# ETAPA 2: Runtime (Imagen pequeña)
FROM mcr.microsoft.com/dotnet/aspnet:10.0
WORKDIR /app
COPY --from=build /app/out .

# Configuración de puerto para .NET 10
EXPOSE 8000
ENV ASPNETCORE_URLS=http://+:8000

ENTRYPOINT ["dotnet", "WebKsApi.dll"]