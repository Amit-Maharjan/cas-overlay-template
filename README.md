# CAS Server (Customized)

This project is a customized implementation of the Apereo CAS server, based on **version 6.1.x**, using **Java 11**.

## Features & Configuration

- **CAS Version**: 6.1.x
- **Java Version**: 11
- **Authentication**: Static user authentication configured via `cas.authn.accept.users`
- **Service Registry**: JSON-based, configured through service definition files
- **Mode**: Standalone execution with a custom configuration directory
- **Client Integration**: Tested with a Spring Boot CAS client using `Cas30ServiceTicketValidator`

### Static User Authentication

Edit `cas.properties` to define demo users:

```properties
cas.authn.accept.users=casuser::Mellon,testuser::1234
```

### Service Registration

Services are registered using JSON files. Example (`service-100.json`):

```json
{
  "@class": "org.apereo.cas.services.RegexRegisteredService",
  "serviceId": "http://localhost:8080/.*",
  "name": "Spring Boot CAS Client",
  "id": 100,
  "evaluationOrder": 1
}
```

> ⚠️ The JSON filename should match the pattern: `[name]-[id].json`

### Configuration Directory

All configuration is read from:

```
/Users/amitmaharjan/Coding/CAS_Project/cas-server/src/main/resources/etc/cas
```

Ensure this path exists and includes:
- `cas.properties`
- `services/` folder with registered service JSON files

## Running the Server

Use the following command to start the CAS server:

```bash
./gradlew run \
  -Dorg.gradle.java.home="$JAVA11_HOME" \
  -Pargs='-Dcas.standalone.configurationDirectory=/Users/amitmaharjan/Coding/CAS_Project/cas-server/src/main/resources/etc/cas'
```

## Notable Runtime Logs

Some important logs to be aware of during startup:

```log
CAS is configured to accept a static list of credentials for authentication...
Ticket registry encryption/signing is turned off...
Generated encryption and signing keys should be added to CAS settings:
- cas.tgc.crypto.encryption.key
- cas.tgc.crypto.signing.key
- cas.webflow.crypto.encryption.key
- cas.webflow.crypto.signing.key
```

## Status Messages

You should see logs like the following after a successful login and service ticket validation:

```log
Authenticated principal [testuser]...
Granted service ticket for [http://localhost:8080/login/cas]...
SERVICE_TICKET_VALIDATE_SUCCESS
```

---

**Note:** This setup is intended for development or demonstration only. For production environments, replace static authentication and memory-based registries with persistent and secure alternatives.