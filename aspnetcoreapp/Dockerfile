# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 5000



# syntax=docker/dockerfile:1
FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
    
# Copy csproj and restore as distinct layers
COPY *.csproj ./

#COPY . ./
RUN dotnet restore
    
# Copy everything else and build
# COPY ../engine/examples ./
COPY . .
WORKDIR /src/.
RUN dotnet build -c Release -o /app/build

FROM build AS publish
RUN dotnet publish -c Release -o /app/publish
    
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "aspnetcoreapp.dll"]