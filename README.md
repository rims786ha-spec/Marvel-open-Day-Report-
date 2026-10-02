# Marvel-open-Day-Report-
DecepKube Report
# Kubernetes-Based Cyber Deception and Runtime Threat Detection with Automated Containment

**Project Type:** Cybersecurity Prototype  
**Domain:** Cybersecurity, Cloud-Native Security, Container Security  
**Development Environment:** Ubuntu Linux, Minikube  
**Project Status:** Prototype developed and tested during implementation

---

## 1. Project Overview

The project aims to develop a cyber-deception system that uses a decoy application to identify suspicious activities in a containerized environment. The system integrates runtime threat detection, security event processing, centralized logging, a monitoring dashboard, and a workload containment mechanism.

The prototype is built using Docker, Kubernetes, Falco, FastAPI, PostgreSQL, and React. Falco monitors runtime activity and generates security events when configured detection rules are triggered. These events are processed by the backend, stored in the database, and presented through a web-based administrative dashboard.

A response engine was also developed to support the containment of suspicious workloads. The current implementation provides a foundation for further development of automated threat detection and response in Kubernetes environments.

## 2. Problem Statement

Containerized applications are widely used in modern software systems, but suspicious activity within running containers can pose security risks. Detecting unexpected command execution and responding to potentially compromised workloads are important aspects of container security.

A monitoring system that only records security events may not be sufficient when a suspicious workload requires immediate attention or isolation.

This project addresses the problem by combining a controlled decoy environment with runtime monitoring, alert generation, centralized event storage, and a prototype containment mechanism.

The primary objective is to demonstrate how these components can work together to identify suspicious behaviour and support a timely security response.

## 3. Target Audience

The intended users and beneficiaries of the project include:

- **Cybersecurity students and researchers:** To understand runtime threat detection, deception techniques, and container security.
- **System administrators:** To monitor suspicious activities in containerized workloads.
- **Security analysts:** To review security alerts and investigate recorded events.
- **Developers and cloud engineers:** To explore security monitoring and containment in Kubernetes environments.

The current implementation is an educational and experimental prototype rather than a production-ready enterprise security platform.

## 4. Project Objectives

The main objectives are:

1. Develop and deploy a decoy application in a Kubernetes environment.
2. Monitor runtime activities using Falco.
3. Configure detection rules for suspicious behaviour, including shell execution within the decoy.
4. Forward security events to a backend for processing.
5. Store alerts and logs in a PostgreSQL database.
6. Develop a web-based dashboard to view alerts, logs, users, and system status.
7. Implement and test a prototype mechanism for isolating a suspicious workload.
8. Establish a foundation for future improvements in cloud-native security.

## 5. Project Scope

### 5.1 Included in the Current Prototype

- Containerized decoy application.
- Local Kubernetes deployment using Minikube.
- Runtime monitoring using Falco.
- Custom Falco detection rule for suspicious shell activity in the decoy.
- Falco event forwarding through an adapter.
- Backend APIs developed using FastAPI.
- PostgreSQL storage for alerts, logs, and user records.
- React-based administrative dashboard.
- Prototype response engine for workload containment.
- Testing of the detection, alerting, and response workflow.

### 5.2 Outside the Current Scope

The following are not claimed as completed features:

- AI/ML-based threat classification.
- Advanced attacker profiling or behavioural analytics.
- Deployment on a production cloud Kubernetes cluster.
- Multiple specialised decoys such as SSH, database, and cloud-service honeypots.
- Integration with commercial SIEM/SOAR platforms.
- Enterprise-scale multi-node monitoring.
- Guaranteed prevention of all attacks or lateral movement.
- Production-grade incident response and recovery.

These can be considered for future development.

## 6. Technical Architecture

The system consists of five main layers.

### 6.1 Decoy and Kubernetes Layer

The decoy application is containerized and deployed as a Kubernetes workload. Minikube provides the local Kubernetes cluster used for development and testing.

The resources are organised within the `cyber-deception` namespace.

### 6.2 Runtime Detection Layer

Falco monitors runtime activity and evaluates events against configured rules. A custom rule, referred to during development as **“DecepKube Shell in Decoy,”** was used to detect shell execution in the decoy environment.

When the configured behaviour is detected, Falco generates a security event.

### 6.3 Event Processing Layer

A Falco adapter forwards events to the backend through HTTP requests. The backend processes the incoming events and makes security information available to the application.

The response engine handles the containment logic.

### 6.4 Database Layer

PostgreSQL stores application and security-related information. SQLAlchemy is used for database interaction, while Alembic is used to manage database schema migrations.

The database includes tables for:

- Users
- Alerts
- Logs

### 6.5 Dashboard Layer

The React-based administrative portal provides a visual interface for monitoring the system. It communicates with the backend through REST APIs.

The overall architecture is represented below:

```text
                 Suspicious Activity
                         |
                         v
                 +---------------+
                 |  Decoy Pod    |
                 |  Kubernetes   |
                 +-------+-------+
                         |
                         v
                 +---------------+
                 |     Falco     |
                 | Runtime Rules |
                 +-------+-------+
                         |
                         v
                 +---------------+
                 | Falco Adapter |
                 +-------+-------+
                         |
                         v
                 +---------------+
                 |   FastAPI     |
                 |    Backend    |
                 +---+-------+---+
                     |       |
                     v       v
              +----------+  +----------------+
              |PostgreSQL|  | Response Engine|
              |Alerts    |  | Containment    |
              |Logs      |  +----------------+
              |Users     |
              +----+-----+
                   |
                   v
           +------------------+
           | React Dashboard  |
           | Alerts and Logs  |
           +------------------+
```

## 7. Tools and Technologies Used

| Technology | Purpose |
|---|---|
| Ubuntu Linux | Development environment |
| Docker | Containerization |
| Kubernetes | Container orchestration |
| Minikube | Local Kubernetes cluster |
| kubectl | Kubernetes resource management and debugging |
| Falco | Runtime security monitoring |
| Falco rules | Detection of configured suspicious activities |
| Python | Backend, adapter, and response logic |
| FastAPI | Backend REST API |
| PostgreSQL | Persistent data storage |
| SQLAlchemy | Database ORM |
| Alembic | Database migrations |
| React | Frontend development |
| Vite | Frontend development and build tooling |
| React Router | Dashboard page navigation |
| YAML | Kubernetes resource configuration |

## 8. Implementation Details

### 8.1 Containerization and Kubernetes Deployment

The decoy application and supporting services were organised as Kubernetes workloads. Minikube was used to run the cluster locally.

Kubernetes commands were used to inspect pods, deployments, services, namespaces, and application logs during development and troubleshooting.

### 8.2 Runtime Threat Detection

Falco was configured to monitor runtime events. A custom detection rule was used to identify shell execution in the decoy.

The detection workflow was tested by generating shell activity in the decoy environment and observing the resulting security events.

### 8.3 Falco Adapter

A custom adapter was developed to receive and forward Falco events to the backend.

During testing, the adapter received HTTP requests at its `/falco` endpoint. Its logs were used to troubleshoot event forwarding and repeated requests.

### 8.4 Backend Development

The backend was developed using FastAPI. The administrative portal uses the following API endpoints:

| Endpoint | Purpose |
|---|---|
| `GET /api/alerts` | Retrieve security alerts |
| `GET /api/logs` | Retrieve application and security logs |
| `GET /api/users` | Retrieve user records |
| `GET /api/system/status` | Check backend and database status |
| `GET /api/system/version` | Retrieve application version |

The system status endpoint was configured to report backend and database health.

### 8.5 Database Implementation

PostgreSQL was configured for persistent storage. The database schema includes models for users, alerts, and logs.

SQLAlchemy was used to connect the application to the database, and Alembic was used to manage migrations.

SQL queries were used to inspect alert and log records during testing.

### 8.6 Administrative Dashboard

The frontend was developed using React and Vite, with React Router for navigation.

The portal includes the following pages:

- Dashboard
- Users
- Alerts
- Logs
- Settings

The frontend was connected to the backend APIs to display application data and system status.

### 8.7 Response and Containment

A response engine was developed to handle containment actions. The implemented containment logic includes scaling the decoy Deployment to zero replicas.

This action is intended to stop the decoy workload from running. It is a workload-level containment mechanism and does not, by itself, establish that the entire Kubernetes cluster or an attacker has been fully isolated.

The response engine and its interaction with the detection workflow were tested during development.

## 9. Testing and Validation

The following activities were carried out during implementation:

| Test | Purpose / Observation |
|---|---|
| Kubernetes resource inspection | Checked pods and other resources in the project namespace. |
| Suspicious shell execution | Generated shell activity in the decoy to test the detection rule. |
| Falco event monitoring | Inspected runtime security events. |
| Adapter testing | Examined incoming requests and adapter logs. |
| Backend API testing | Checked alert, log, user, and system-status endpoints. |
| Database inspection | Queried PostgreSQL tables to inspect alerts and logs. |
| Dashboard integration | Connected frontend pages to backend APIs. |
| Containment testing | Tested the response logic that scales the decoy Deployment to zero replicas. |

During development, repeated event requests and an increasing number of alert and log records were observed. These required debugging of the adapter, response engine, and database behaviour.

The project therefore demonstrates the implementation of the main workflow, but further testing is required to establish reliable operation across repeated demonstrations and different attack scenarios.

## 10. Expected Impact

The project demonstrates a practical approach to learning and experimenting with container security.

Its potential benefits include:

- Improving visibility into suspicious runtime activity.
- Providing a central location for reviewing alerts and logs.
- Demonstrating the relationship between detection and response.
- Helping students understand Kubernetes security through practical implementation.
- Providing a foundation for experiments involving deception and workload containment.

The current prototype has not been evaluated through a formal performance study. Quantitative improvements in detection accuracy, response time, or attack prevention have therefore not been established.

## 11. Challenges and Mitigation Strategies

| Challenge | Mitigation / Approach |
|---|---|
| Kubernetes resource and pod issues | Used `kubectl` commands, pod descriptions, and logs for troubleshooting. |
| Falco event forwarding | Inspected adapter logs and HTTP requests. |
| Repeated alerts and logs | Queried PostgreSQL and examined event-processing behaviour. |
| Backend and database connectivity | Checked API responses and system-status endpoints. |
| Containment behaviour | Examined the response engine and Kubernetes Deployment scaling logic. |
| Reliable demonstration | Requires a repeatable startup, testing, and cleanup procedure. |

## 12. Timeline and Resources

### Resources Used

The prototype was developed using a local Linux development environment and open-source technologies.

The principal resources included:

- A laptop running Ubuntu Linux.
- Docker and Minikube.
- Kubernetes and Falco.
- Python-based backend components.
- PostgreSQL.
- React and Vite.

No verified project budget or detailed expenditure record is available, so a monetary cost is not specified.

### Further Development Plan

The following is a proposed development roadmap, not a record of completed work.

| Phase | Proposed Work |
|---|---|
| Month 1 | Stabilise the current workflow and prepare repeatable tests. |
| Month 2 | Improve alert handling, event correlation, and containment safeguards. |
| Month 3 | Evaluate detection and response using defined test scenarios. |
| Months 4–6 | Explore additional decoys, cloud deployment, and advanced analysis. |

The schedule may change depending on testing results, available resources, and project requirements.

## 13. Future Scope

### 13.1 Multiple Deception Environments

Introduce different decoy services, such as web applications, SSH services, and database services, to study a wider range of suspicious interactions.

### 13.2 Improved Detection and Event Correlation

Correlate related events into timelines to make investigation easier and reduce duplicate or disconnected alerts.

### 13.3 AI/ML-Based Behaviour Analysis

Explore machine learning for analysing event patterns and identifying potentially suspicious behaviour. This would require suitable datasets, model evaluation, and testing for false positives.

### 13.4 Safer Automated Containment

Extend the response engine with configurable policies, approval mechanisms, evidence preservation, and recovery procedures.

### 13.5 Cloud and Multi-Node Deployment

Evaluate the system on a cloud-based or multi-node Kubernetes environment to study its scalability and operational requirements.

### 13.6 Security Operations Integration

Explore integration with SIEM or SOAR platforms for centralised monitoring, incident management, and coordinated response.

### 13.7 Performance Evaluation

Conduct controlled experiments to measure:

- Detection latency.
- Alert-processing time.
- Containment time.
- False-positive and false-negative rates.
- Resource consumption.
- Reliability across repeated test runs.

These measurements would help determine the effectiveness and limitations of the proposed approach.

## 14. References and Attributions

The implementation uses the following technologies and their official documentation as technical references:

1. Docker Documentation — https://docs.docker.com/
2. Kubernetes Documentation — https://kubernetes.io/docs/
3. Minikube Documentation — https://minikube.sigs.k8s.io/docs/
4. Falco Documentation — https://falco.org/docs/
5. FastAPI Documentation — https://fastapi.tiangolo.com/
6. PostgreSQL Documentation — https://www.postgresql.org/docs/
7. SQLAlchemy Documentation — https://docs.sqlalchemy.org/
8. Alembic Documentation — https://alembic.sqlalchemy.org/
9. React Documentation — https://react.dev/
10. Vite Documentation — https://vite.dev/

These references document the technologies used; they are not claims of endorsement by the respective projects.

## 15. Conclusion

The project demonstrates a prototype cyber-deception and runtime threat detection system for a Kubernetes environment. It combines a decoy workload, Falco-based detection, event forwarding, a FastAPI backend, PostgreSQL storage, and a React administrative dashboard.

A response engine was also developed to support workload containment by scaling the decoy Deployment to zero replicas.

The work provided practical experience in containerisation, Kubernetes administration, runtime security monitoring, backend development, database integration, and frontend implementation.

The current system is a prototype and requires further reliability testing, security evaluation, and operational improvements before it can be considered for production use.

Future work will focus on improving event correlation, evaluating detection and containment performance, introducing additional deception environments, and exploring cloud deployment and intelligent security analysis.

The long-term objective is to develop a more reliable platform that helps security teams observe suspicious activity, investigate events, and respond to threats in containerized environments.
