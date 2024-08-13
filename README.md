# aspdotnet-sample-app
Sample ASPDOTNET App

## DOTNET Setup commands for UBUNTU 20.04 and .Netv3.1

```
wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
```
```
sudo dpkg -i packages-microsoft-prod.deb
```
```
apt update && apt install dotnet-sdk-3.1
```
```
dotnet --info
```


## Local Build and deploy commands

```
dotnet new webapp -o aspnetcoreapp
```

```
cd aspnetcoreapp/
```
```
dotnet watch run
```

## Deploy using a Dockerfile

### Building Docker Image Locally
```
docker build -t <imagename> .
```

### Running docker container using Above Build Image
```
docker run -p 5000:5000 -e ASPNETCORE_URLS=http://*:5000 <imagename>
```

## TESTING
```
http://<server_public_ip>:5000
```

# APP SPEC FILE
```
ingress:
  rules:
  - component:
      name: aspdotnet-sample-app
    match:
      path:
        prefix: /
name: aspdotnet-sample-app
region: nyc
services:
- dockerfile_path: aspnetcoreapp/Dockerfile
  envs:
  - key: ASPNETCORE_URLS
    scope: RUN_AND_BUILD_TIME
    value: http://*:5000
  github:
    branch: main
    deploy_on_push: true
    repo: abuttDO/aspdotnet-sample-app
  http_port: 5000
  instance_count: 1
  instance_size_slug: basic-xs
  name: aspdotnet-sample-app
  source_dir: aspnetcoreapp
```
