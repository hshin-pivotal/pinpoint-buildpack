# Pinpoint Buildpack 
## purpose
This buildpack is intened to seperate pinpoint buildpack from java_buildpack_offline which is maintained by OSS, so that you can keep update java_buildpack_offline regardless of pinpoint buildpack version.

## how it works
This is a non-final buildpack for Cloud Foundry that provides integration with Pinpoint agent(https://naver.github.io/pinpoint)
This buildpack works with final buildpack that supports multi buildpack such as java-buildpack(https://github.com/cloudfoundry/java-buildpack/blob/master/docs/framework-multi_buildpack.md#multiple-buildpack-integration-api)


## how to build buildpack
- prepare your pinpoint agent zip and pinpoint.config. refer to https://github.com/myminseok/pinpoint_agent_repo
- edit buildpack configuration.
```
git clone https://github.com/myminseok/pinpoint-buildpack
cd pinpoint-buildpack

vi lib/config.yml . 
vi bin/supply

./upload

```

## how to deploy apps


just use -b option for multiple buildpacks when you push your app.

```sh
git clone  https://github.com/myminseok/spring-music
cd spring-music
./gradlew clean assemble
```

### manifest.yml

```
---
applications:
- name: spring-music
  memory: 1G
  buildpacks:
  - https://github.com/myminseok/pinpoint-buildpack
  - java_buildpack_offline
  random-route: true
  #path: build/libs/spring-music.jar
  path: ./spring-music.jar
  env:
    PINPOINT_AGENT_PACKAGE_DOWNLOAD_URL: https://github.com/naver/pinpoint/releases/download/1.8.2/pinpoint-agent-1.8.2.tar.gz
    PINPOINT_CONFIG_URL: https://raw.githubusercontent.com/myminseok/pinpoint_agent_repo/master/pinpoint.config-1.8.2
```
- PINPOINT_AGENT_PACKAGE_DOWNLOAD_URL: (optional, default: https://github.com/myminseok/pinpoint_agent_repo/blob/master/pinpoint-agent-1.8.2-SNAPSHOT.tar.gz?raw=true )url for pinpoint-agent-1.7.4-SNAPSHOT.tar.gz, git binary from https://github.com/naver/pinpoint/releases
- PINPOINT_CONFIG_URL: configuraton for pinpoint. profiler.collector.ip should point the pinpoint server. (optional, default: pinpoint.config packaged in PINPOINT_AGENT_PACKAGE_DOWNLOAD_URL)

```
cf push -f manifest.yml -b pinpoint_buildpack -b java_buildpack_offline -p build/libs/spring-music.jar
```

See https://docs.cloudfoundry.org/buildpacks/understand-buildpacks.html for buildpack basics. This is an 
intermediate buildpack using only the bin/supply script.

This buildpack is inspired by https://github.com/cf-platform-eng/eureka-registrar-sidecar



## pinpoint server setup (docker on ubuntu)

### requirements
- https://docs.docker.com/compose/compose-file/
- Docker Engine release: 18.02.0+
- Docker compose: 3.6+

### install docker-compose
- https://docs.docker.com/compose/install/
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

docker-compose --version
=> docker-compose version 1.23.2, build 1110ad01

```

### run pinpoint server(docker)
- https://github.com/naver/pinpoint-docker/releases
```
wget https://github.com/naver/pinpoint-docker/archive/1.8.2.tar.gz

tar xf  1.8.2.tar.gz
cd pinpoint-1.8.2
sudo docker-compose up

```

###  pinpoint web
```
open http://<pinpoint-server-ip>:8079
```  
  
###  reference.
- https://naver.github.io/pinpoint/1.7.3/installation.html
- https://github.com/naver/pinpoint/blob/master/doc/installation.md




http://10.10.10.199:8079

