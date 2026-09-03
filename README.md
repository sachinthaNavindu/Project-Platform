# Project Platform

A centralized **microservices platform and infrastructure orchestration project** built with Spring Boot and Spring Cloud.

This repository serves as the parent project for the core infrastructure components of the Enterprise Cloud Application (ECA) microservices architecture:

* **Config Server** — Centralized configuration management
* **Service Registry** — Service discovery using Netflix Eureka
* **API Gateway** — Centralized API routing and entry point

The project combines these infrastructure services into a single Maven multi-module project and provides a centralized way to build and manage the platform.

---

## 🚀 Overview

Modern microservices applications typically consist of multiple independently deployable services.

Managing infrastructure components separately can make local development, deployment, and service management more complicated.

The **Project Platform** provides a centralized foundation for the microservices ecosystem.

```text
                         ┌─────────────────────────┐
                         │      Client / UI        │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │      API Gateway        │
                         │         :7000            │
                         └────────────┬────────────┘
                                      │
                                      │ Service Discovery
                                      ▼
                         ┌─────────────────────────┐
                         │    Service Registry     │
                         │      Eureka :9001       │
                         └────────────┬────────────┘
                                      │
                       ┌──────────────┼──────────────┐
                       │              │              │
                       ▼              ▼              ▼
                ┌────────────┐ ┌────────────┐ ┌────────────┐
                │  Student   │ │  Program   │ │ Enrollment │
                │  Service   │ │  Service   │ │  Service   │
                └────────────┘ └────────────┘ └────────────┘


                         ┌─────────────────────────┐
                         │      Config Server      │
                         │         :9000            │
                         └─────────────────────────┘
```

The platform infrastructure follows a layered architecture:

```text
Configuration
     │
     ▼
Service Discovery
     │
     ▼
API Gateway
     │
     ▼
Business Microservices
```

---

## 🏗️ Platform Components

The project currently contains three Maven modules:

| Component        | Responsibility                    |   Port |
| ---------------- | --------------------------------- | -----: |
| Config Server    | Centralized configuration         | `9000` |
| Service Registry | Service registration & discovery  | `9001` |
| API Gateway      | Request routing & API entry point | `7000` |

These modules are explicitly configured in the root `pom.xml`.

### Config Server

Provides centralized configuration to the distributed services.

```text
Config Server
     │
     ├── API Gateway configuration
     ├── Service Registry configuration
     ├── Student Service configuration
     ├── Program Service configuration
     └── Enrollment Service configuration
```

---

### Service Registry

Provides service discovery using **Netflix Eureka**.

Microservices register themselves with the registry and can subsequently be discovered using their logical service names.

```text
STUDENT-SERVICE
PROGRAM-SERVICE
ENROLLMENT-SERVICE
API-GATEWAY
```

---

### API Gateway

Acts as the single entry point for external clients.

Instead of clients communicating directly with individual microservices:

```text
Client
  │
  ├── Student Service
  ├── Program Service
  └── Enrollment Service
```

requests are centralized through:

```text
Client
   │
   ▼
API Gateway
   │
   ▼
Microservices
```

This provides a cleaner and more controlled architecture.

---

## 📦 Multi-Module Maven Architecture

The root project is configured as a Maven parent project.

```xml
<packaging>pom</packaging>
```

and currently contains:

```text
project-platform
│
├── api-gateway
├── config-server
└── service-registry
```

The root `pom.xml` defines:

```text
lk.ijse.eca
    │
    └── project-platform
            │
            ├── api-gateway
            ├── config-server
            └── service-registry
```

This allows the infrastructure modules to be managed from one parent project.

---

## 📂 Project Structure

```text
Project-Platform/
│
├── api-gateway/
│   └── API Gateway Spring Boot application
│
├── config-server/
│   └── Spring Cloud Config Server
│
├── service-registry/
│   └── Netflix Eureka Service Registry
│
├── logs/
│   └── Application log files
│
├── .gitmodules
│
├── ecosystem.config.js
│
├── pom.xml
│
└── README.md
```

The root repository currently contains the three infrastructure components as modules along with the PM2 ecosystem configuration and application logs directory.

---

## 🛠 Technology Stack

| Technology           | Purpose                       |
| -------------------- | ----------------------------- |
| Java                 | Application runtime           |
| Spring Boot          | Application framework         |
| Spring Cloud         | Microservices infrastructure  |
| Spring Cloud Config  | Centralized configuration     |
| Spring Cloud Gateway | API Gateway                   |
| Netflix Eureka       | Service discovery             |
| Maven                | Multi-module build management |
| PM2                  | Process management            |
| Git                  | Version control               |

The root Maven project uses the group ID `lk.ijse.eca`, artifact ID `project-platform`, version `1.0.0`, and `pom` packaging.

---

## 🔄 Platform Request Flow

A typical request flows through the platform as follows:

```text
                    Client
                      │
                      │ HTTP Request
                      ▼
              ┌───────────────┐
              │ API Gateway   │
              │    :7000      │
              └───────┬───────┘
                      │
                      │ Discover Service
                      ▼
              ┌───────────────┐
              │ Eureka Server │
              │    :9001      │
              └───────┬───────┘
                      │
             ┌────────┼────────┐
             │        │        │
             ▼        ▼        ▼
         Student   Program  Enrollment
         Service   Service    Service
```

Configuration is provided separately through:

```text
              ┌────────────────┐
              │ Config Server  │
              │     :9000      │
              └───────┬────────┘
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
    Gateway       Registry      Microservices
```

---

## ⚙️ Prerequisites

Before running the platform, install:

* Java
* Maven
* Git
* Node.js
* PM2

The project includes Maven Wrapper files within the individual Spring Boot modules, allowing the modules to be built without relying exclusively on a globally installed Maven version.

---

## 📥 Clone the Repository

```bash
git clone https://github.com/sachinthaNavindu/Project-Platform.git
```

Navigate into the project:

```bash
cd Project-Platform
```

---

## 🏗️ Build the Complete Platform

Because this is a Maven multi-module project, the infrastructure components can be built from the root directory.

```bash
mvn clean package
```

This builds the modules defined in the root Maven project:

```text
api-gateway
config-server
service-registry
```

The root project defines these modules directly in its `pom.xml`.

---

## ▶️ Running the Platform

The services should be started in dependency order.

### Recommended startup sequence

```text
1. Config Server
       │
       ▼
2. Service Registry
       │
       ▼
3. API Gateway
```

### Step 1 — Config Server

Start the Config Server first.

```text
http://localhost:9000
```

The other infrastructure components depend on centralized configuration.

---

### Step 2 — Service Registry

Start the Eureka Service Registry.

```text
http://localhost:9001
```

The registry provides service discovery for the platform.

---

### Step 3 — API Gateway

Start the API Gateway.

```text
http://localhost:7000
```

The gateway then communicates with services discovered through Eureka.

---

## ⚡ Process Management with PM2

The project includes an `ecosystem.config.js` file for running the Java infrastructure services through **PM2**.

The configuration defines three applications:

```text
config-server
service-registry
api-gateway
```

Each application runs its corresponding Spring Boot JAR and writes logs to the `logs/` directory.

### Build the applications first

```bash
mvn clean package
```

Then start the platform using:

```bash
pm2 start ecosystem.config.js
```

---

## 📊 PM2 Process Management

After starting the applications:

```bash
pm2 list
```

You should see:

```text
┌───────────────────┐
│ config-server     │
│ service-registry  │
│ api-gateway       │
└───────────────────┘
```

The current PM2 configuration starts:

```text
Config Server
target/Config-Server-1.0.0.jar

Service Registry
target/Service-Registry-1.0.0.jar

API Gateway
target/Api-Gateway-1.0.0.jar
```

and writes their output to:

```text
logs/config-server.log
logs/service-registry.log
logs/api-gateway.log
```

These paths are defined in the repository's `ecosystem.config.js`.

---

## 📝 Viewing Logs

PM2 can be used to monitor the running services.

View all logs:

```bash
pm2 logs
```

View Config Server logs:

```bash
pm2 logs config-server
```

View Service Registry logs:

```bash
pm2 logs service-registry
```

View API Gateway logs:

```bash
pm2 logs api-gateway
```

Application log files are also configured under:

```text
logs/
```

---

## 🔁 Restarting Services

Restart an individual service:

```bash
pm2 restart config-server
```

```bash
pm2 restart service-registry
```

```bash
pm2 restart api-gateway
```

Restart the complete platform:

```bash
pm2 restart all
```

---

## 🛑 Stopping Services

Stop a specific service:

```bash
pm2 stop config-server
```

Stop all platform services:

```bash
pm2 stop all
```

Remove all processes from PM2:

```bash
pm2 delete all
```

---

## 💾 Persisting PM2 Processes

To save the current PM2 process configuration:

```bash
pm2 save
```

This allows the process manager to restore the configured applications after a restart when PM2 startup integration is configured on the host.

---

## 🔍 Platform Verification

After starting all infrastructure services, verify each component.

### Config Server

```text
http://localhost:9000
```

### Eureka Service Registry

```text
http://localhost:9001
```

The Eureka dashboard should display registered services once the clients have successfully connected.

### API Gateway

```text
http://localhost:7000
```

The gateway should be able to route requests to registered backend services.

---

## 🧪 Health & Monitoring

The Spring Boot services use management capabilities that can be used to monitor application health.

For example:

```text
http://localhost:9000/actuator/health
```

```text
http://localhost:9001/actuator/health
```

```text
http://localhost:7000/actuator/health
```

Availability of individual endpoints depends on the management configuration of each service.

---

## 🔐 Configuration Management

The platform separates infrastructure configuration from application logic using Spring Cloud Config.

```text
                Config Server
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
     Gateway      Registry    Microservices
```

This provides a centralized approach to configuration management and reduces configuration duplication across services.

Sensitive information such as:

* Database passwords
* API keys
* Authentication secrets
* Cloud credentials

should not be committed directly to source control.

For production deployments, use environment variables or a dedicated secrets-management solution.

---

## 📈 Scalability

The architecture is designed to support horizontal scaling of individual microservices.

For example:

```text
                    Eureka
                      │
              STUDENT-SERVICE
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
      Instance 1  Instance 2  Instance 3
```

The API Gateway can communicate with the logical service name rather than relying on a single fixed server address.

This makes the architecture more suitable for cloud and distributed deployments.

---

## ☁️ Cloud Deployment Architecture

The platform can be deployed as a set of independently managed services.

A typical deployment can look like:

```text
                     Internet
                        │
                        ▼
                ┌──────────────┐
                │ API Gateway  │
                │    :7000     │
                └──────┬───────┘
                       │
                       ▼
               ┌───────────────┐
               │ Eureka Server │
               │    :9001      │
               └───────┬───────┘
                       │
            ┌──────────┼──────────┐
            ▼          ▼          ▼
        Student     Program    Enrollment
        Service     Service      Service

                       ▲
                       │
                ┌──────┴──────┐
                │Config Server│
                │    :9000    │
                └─────────────┘
```

The infrastructure components can be deployed independently while maintaining service discovery and centralized configuration.

---

## 🔗 Repository Components

This parent repository contains the following infrastructure projects:

### API Gateway

Responsible for centralized request routing and client access.

```text
api-gateway/
```

### Config Server

Responsible for centralized configuration management.

```text
config-server/
```

### Service Registry

Responsible for service registration and discovery.

```text
service-registry/
```

Together, these components form the infrastructure layer of the microservices ecosystem.

---

## 📋 Platform Ports

| Component        |   Port | Purpose                   |
| ---------------- | -----: | ------------------------- |
| Config Server    | `9000` | Centralized configuration |
| Service Registry | `9001` | Service discovery         |
| API Gateway      | `7000` | API routing               |

---

## 🧭 Dependency Flow

```text
Config Server
      │
      │ Provides configuration
      ▼
Service Registry
      │
      │ Provides service discovery
      ▼
API Gateway
      │
      │ Routes requests
      ▼
Business Microservices
```

This dependency flow should be considered when starting or deploying the platform.

---

## 🎯 Project Goals

The main goals of the Project Platform are:

* Provide a centralized microservices infrastructure layer
* Simplify configuration management
* Enable dynamic service discovery
* Provide a single API entry point
* Support scalable service communication
* Simplify local and server-side process management
* Provide a consistent structure for the ECA microservices ecosystem

## 👨‍💻 Author

**Sachintha Navindu**
