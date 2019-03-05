# Pinpoint Buildpack 

This is a non-final buildpack for Cloud Foundry that provides integration with Pinpoint agent(https://naver.github.io/pinpoint)

This buildpack works with final buildpack that supports multi buildpack such as java-buildpack(https://github.com/cloudfoundry/java-buildpack/blob/master/docs/framework-multi_buildpack.md#multiple-buildpack-integration-api)

When this buildpack is present in your Cloud Foundry deployment, all you will have to do to register all 
instances of your application with the Eureka discovery server is bind the application to your Spring Cloud
Eureka Servive instance. Instances will then automatically be registered and continue to send heartbeats
as long as the application container is alive when you provide the eureka_registrary_sidecar as an intermediate
buildpack when pushing your application.

```sh
git clone  https://github.com/myminseok/spring-music
cd spring-music
./gradlew clean assemble
cf push -f manifest.yml -b https://github.com/myminseok/pinpoint-buildpack -b java_buildpack_offline -p build/libs/spring-music.jar
```

See https://docs.cloudfoundry.org/buildpacks/understand-buildpacks.html for buildpack basics. This is an 
intermediate buildpack using only the bin/supply script.

This buildpack is inspired by https://github.com/cf-platform-eng/eureka-registrar-sidecar
