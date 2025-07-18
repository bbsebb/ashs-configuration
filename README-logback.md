# Logback Configuration

## Overview

This document describes the logback configuration setup for the microservices in the ASHS backend.

## Common Logback Configuration

To avoid redundant configuration across services, the common logback configuration has been externalized to a shared file:

`backend/configuration/common-logback.xml`

This file contains the shared configuration elements:
- Loki appender configuration
- Console appender configuration
- Root logger configuration

## Service-specific Logback Configuration

Each service has a minimal `logback-spring.xml` file that includes:
1. Spring Boot's standard logging configurations
2. The springProperty declaration for the appName variable
3. An include directive to the common configuration file

Example:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/base.xml"/>
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <springProperty scope="context" name="appName" source="spring.application.name"/>
    
    <!-- Include common logback configuration -->
    <include file="${user.dir}/configuration/common-logback.xml"/>
</configuration>
```

## Path Resolution

The common configuration file is included using `${user.dir}/configuration/common-logback.xml`, where `${user.dir}` refers to the current working directory of the application. This assumes that:

1. The services are run from the backend directory
2. The configuration directory is directly under the backend directory

If the working directory changes in different environments (dev, prod, etc.), you may need to adjust the path accordingly.

## Customization

If a service needs custom logging configuration beyond what's in the common file, you can add service-specific appenders or loggers in the service's `logback-spring.xml` file after the include directive.