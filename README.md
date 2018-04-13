# Grails + Sqreen test app

This is a basic Grails test app (default structure created from `grails create-app test-app`) to validate Sqreen integration.

## Sqreen agent custom Elastic Beanstalk configuration

The following file has been created : `/src/main/eb/.ebextensions/sqreen.config`

```yml
files:
  "/var/lib/sqreen/sqreen-agent.jar" :
    mode: "000755"
    owner: root
    group: root
    source: https://download.sqreen.io/java/sqreen-latest-all.jar
```

The `build.gradle` has been updated to include custom eb config.

```groovy
war {
    from('src/main/eb/.ebextensions') {
        into('.ebextensions')
    }
}
```

## Running the app locally

From your command line: `./gradlew bootRun`

## Running the app on Elastic Beanstalk

From your commande line: `./gradlew assemble`

Create a new Elastic Beanstalk app, based on Tomcat8 and upload the generated war file `build/libs/test-app-0.1.war`.

Once the app is running, change the software config:
1. *JVM Options*, add  `-javaagent:/var/lib/sqreen/sqreen.jar`
2. *Environment Properties*, add `sqreen.token` with your own token provided by Sqreen.
