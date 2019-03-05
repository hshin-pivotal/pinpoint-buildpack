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
cf push -f manifest.yml -b pinpoint_buildpack -b java_buildpack_offline -p build/libs/spring-music.jar
```

See https://docs.cloudfoundry.org/buildpacks/understand-buildpacks.html for buildpack basics. This is an 
intermediate buildpack using only the bin/supply script.

This buildpack is inspired by https://github.com/cf-platform-eng/eureka-registrar-sidecar
