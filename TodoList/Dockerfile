# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 5253
EXPOSE 80



# syntax=docker/dockerfile:1
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
    
# Copy csproj and restore as distinct layers
COPY ["TodoList.csproj", "."]

#COPY . ./
RUN dotnet restore "TodoList.csproj"
    
# Copy everything else and build
# COPY ../engine/examples ./
COPY . .
WORKDIR /src/.
RUN dotnet build "TodoList.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TodoList.csproj" -c Release -o /app/publish
    
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS http://*:5253
ENTRYPOINT ["dotnet", "TodoList.dll"]
