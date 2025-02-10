# Spring Cloud Config Repository

config-service is a repository which stores configuration files for applications using Spring Cloud Config, enabling centralized management of properties across different environments.

## Structure

Configuration files follow this pattern:

```
<application-name>-<environment>.yml
<application-name>.yml
```

### Configuration Files

- **refresh-batch** (stored in `refresh-batch/` folder):
    - `refresh-batch.yml` (default)
    - `refresh-batch-dev.yml` (development)
    - `refresh-batch-local.yml` (local)
    - `refresh-batch-prod.yml` (production)

All other configurations are in the root directory:

- **config-batch**

    - `config-batch.yml`, `config-batch-dev.yml`, `config-batch-local.yml`, `config-batch-prod.yml`

- **task-server**

    - `task-server.yml`, `task-server-dev.yml`, `task-server-local.yml`, `task-server-prod.yml`

## How It Works

- `<application-name>.yml` provides common properties.
- `<application-name>-<environment>.yml` overrides settings for specific environments.
- Applications request configurations from the Spring Cloud Config Server based on their name and active profile.

## Usage with Spring Cloud Config Server

Set up the Config Server URL in `application.yml` or `bootstrap.yml`:

```yaml
spring:
  config:
    import: "optional:configserver:http://localhost:8888"
```

Define the application name and active profile:

```yaml
spring:
  application:
    name: task-server
  profiles:
    active: dev
```

For example, `task-server` with profile `dev` fetches `task-server-dev.yml`.

## Refreshing Configuration

Reload properties at runtime with:

```sh
curl -X POST http://localhost:8080/actuator/refresh
```

Ensure Spring Boot Actuator and Spring Cloud Config dependencies are included.

---

This repository provides a structured approach for managing configurations across multiple environments, improving maintainability and consistency.

