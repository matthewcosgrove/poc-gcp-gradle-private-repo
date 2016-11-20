### Purpose

This project is created as a PoC of deploying a Spring Boot app with a Gradle set up to a private repo on GCP as created by https://github.com/renaudcerrato/appengine-maven-repository

It was created by selecting a Gradle project from http://start.spring.io/ (it is bare bones without any additional dependencies added)

### Gradle Configuration

The repo URL and creds have been moved to `~/.gradle/gradle.properties` and the appropriate config from [gradle.build](gradle.build) to enable a Maven deploy (achieved in Gradle by `uploadArchives`) is 

```gradle
apply plugin: 'java'
apply plugin: 'maven'

...

uploadArchives {
    repositories {
        mavenDeployer {
            repository(url: mavenRepoGoogleCloudUrl) {
                authentication(userName: mavenRepoGoogleCloudUser, password: mavenRepoGoogleCloudPassword)
            }
        }
    }
}

group = 'poc.mc'
version = '0.0.2-SNAPSHOT'

jar {
	baseName = 'poc-gcp-gradle-private-repo'
}
```
Note that the `apply plugin: 'maven'` was added as it was not part of the Spring Boot skeleton app.

The artifact can now be deployed with

```bash
$ ./gradlew upload
```
