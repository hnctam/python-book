# Cloud Foundry

## What is Cloud Foundry

Cloud Foundry is an open-source platform as a service (PaaS) that provides you with a choice of clouds, developer frameworks, and application services. It is open source and it is governed by the Cloud Foundry Foundation. The original Cloud Foundry was developed by VMware and currently it is managed by Pivotal, a joint venture company by GE, EMC and VMware.

Now since Cloud Foundry is open source product many popular organizations currently provides this platform separately and below are the list of current certified providers.

* Pivotal Cloud Foundry
* IBM Bluemix
* HPE Helion Stackato 4.0
* Atos Canopy
* CenturyLink App Fog
* GE Predix
* Huawei FusionStage
* SAP Cloud Platform
* Swisscom Application Cloud

## Cloud Foundry Installation for Windows

* Download the [CF Windows installer](https://cli.run.pivotal.io/stable?release=windows64&source=github). It will prompt for the download. Save the zip file distribution.
* Unpack the zip file to a suitable place in your workstation.
* After successfully unzip operation, double cick on the cf CLI executable.
* When prompted, click Install, then Close. Here are the sample steps for the same. This is very straight froward, you can select the default values.

## Setup PWS Console
* Create one account in pivotal in order to deploy our application in Pivotal Cloud Foundry Platform.
* [Register](https://account.run.pivotal.io/z/uaa/sign-up) in the below page to start with the sign up process

![alt](https://howtodoinjava.com/wp-content/uploads/2017/07/pivotal_console_signup.jpg)

* Add organization & space

![alt](https://howtodoinjava.com/wp-content/uploads/2017/07/pcf_console_without_any_apps.jpg)

## CLI

```bash
# Login
cf login -a api.run.pivotal.io
# Logout
cf logout
# Push boot jar to PCF
cf push demo-service -p target\demo-service-0.0.1-SNAPSHOT.jar
```

## Deploy Spring Boot Application to Cloud Foundry Platform

* Clone source from https://github.com/hnctam/spring-cloud
* Goto `demo-service`
* Execute `mvn clean install`
* Login to cloud foundry
