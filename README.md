# TTS Attendance — Cloud Engineering Case Study

> **Private production project** developed for Technocrat Technical Services.
> This repository contains sanitized documentation and architecture information only. The application source code, company data, credentials, and other proprietary information are not included.

## Overview

TTS Attendance is a cloud-based attendance and workforce management platform developed to automate attendance collection, workforce tracking, overtime calculation, timesheets, and reporting.

The platform integrates biometric attendance devices located across company sites with a cloud-hosted application, while also supporting browser/mobile-based attendance for employees who work away from biometric devices.

The system is designed to replace manual attendance processing and spreadsheet-based workflows with a centralized cloud platform.

---

## My Role

**Cloud Engineer / Infrastructure Engineer**

My responsibilities included designing, deploying, configuring, and troubleshooting the cloud infrastructure supporting the application.

Key areas of responsibility:

* AWS infrastructure setup and administration
* Linux server administration
* Docker-based application deployment
* Cloud networking and connectivity
* Secure connectivity between AWS and the organization's office network
* PostgreSQL database infrastructure
* HTTPS and reverse proxy configuration
* AWS service integration
* Infrastructure troubleshooting
* Monitoring and operational support
* Application deployment and environment configuration
* Security and access management

---

## High-Level Architecture

```text
                    OFFICE / FACTORY
                           │
              ┌────────────┴────────────┐
              │                         │
       Hikvision Devices          Employee Browser
              │                         │
              └────────────┬────────────┘
                           │
                    Secure Connectivity
                           │
                       Twingate
                           │
═══════════════════════════╪══════════════════════════
                           │
                      AWS Environment
                           │
                    ┌──────▼──────┐
                    │    EC2      │
                    │             │
                    │   Docker    │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
              ▼            ▼            ▼
           Backend      Frontend     Caddy
          Go / Gin     Next.js       HTTPS
              │
              │
              ▼
        PostgreSQL Database
              │
        ┌─────┴──────────┐
        ▼                ▼
       S3           Rekognition
```

> The architecture shown above is intentionally simplified and does not expose private network addresses, credentials, or internal infrastructure details.

---

## Technology Stack

### Cloud & Infrastructure

* Amazon Web Services (AWS)
* Amazon EC2
* Amazon S3
* AWS Rekognition
* PostgreSQL
* Linux

### Application Infrastructure

* Docker
* Caddy
* Go
* Gin
* Next.js
* React
* TypeScript

### Networking & Security

* Twingate
* HTTPS / TLS
* Role-Based Access Control
* Audit Logging
* Private network connectivity

---

## Cloud Infrastructure

The application is hosted on AWS using a Linux-based EC2 environment.

The application components are containerized using Docker, allowing the frontend and backend services to be deployed and managed consistently.

Caddy is used as the reverse proxy and HTTPS entry point for the application.

The architecture separates application services from persistent database storage and integrates additional AWS services where required.

---

## Hybrid Connectivity

One of the key infrastructure challenges was connecting cloud-hosted application infrastructure with biometric devices located inside the organization's private office and factory networks.

The biometric devices were not intended to be directly exposed to the public internet.

A secure connectivity layer using Twingate was implemented to provide controlled communication between the AWS environment and the private office network.

This allowed the cloud application to communicate with the attendance devices while keeping the devices within the organization's internal network.

---

## Attendance Data Flow

The general attendance flow is:

```text
Biometric Device
       │
       ▼
Private Office Network
       │
       ▼
Secure Connectivity
       │
       ▼
AWS Application
       │
       ▼
Attendance Processing
       │
       ▼
PostgreSQL
       │
       ▼
Web Dashboard / Reports
```

Raw attendance events are processed into meaningful attendance records such as:

* Present
* Absent
* Late
* Missing punch
* Overtime
* Shift-based attendance

---

## Browser / Mobile Attendance

For employees who do not work near a biometric device, the platform provides browser/mobile-based attendance.

The workflow includes:

1. Employee initiates attendance from a browser or mobile device.
2. A selfie is captured.
3. Location information is validated against the assigned work site.
4. AWS Rekognition is used for face verification.
5. The attendance event is recorded in the system.

The platform also supports attendance from an external work location, such as a client site, when the employee provides an appropriate reason or note.

---

## AWS Rekognition Integration

AWS Rekognition is used as part of the face verification workflow.

The system validates that the submitted image contains a suitable face and performs identity verification before accepting the attendance event.

This provides an additional layer of protection against attendance being recorded on behalf of another employee.

---

## Application Deployment

The production application is containerized using Docker.

The deployment consists broadly of:

```text
Internet
   │
   ▼
Caddy / HTTPS
   │
   ├──────────────► Next.js Frontend
   │
   └──────────────► Go / Gin Backend
                          │
                          ▼
                     PostgreSQL
```

Containerization simplifies deployment and provides consistent application environments.

---

## Production Troubleshooting Example

A real production synchronization issue occurred when attendance data from the biometric devices stopped reaching the cloud application.

The application itself was operational, but the attendance synchronization had stopped.

The investigation identified the issue in the secure connectivity layer between the AWS environment and the office network.

After restoring the connectivity layer, communication with the biometric devices resumed and attendance synchronization returned to normal.

This incident provided practical experience with:

* Production troubleshooting
* Network connectivity
* Hybrid infrastructure
* Service dependency analysis
* Monitoring application behavior
* Isolating infrastructure failures from application failures

---

## Security Considerations

Security considerations implemented or addressed in the platform include:

* HTTPS/TLS for web traffic
* Controlled private network connectivity
* Role-based access control
* Restricted infrastructure access
* Secure credential handling
* Audit logging
* Database access controls
* Avoiding direct public exposure of biometric devices

No credentials, private keys, employee information, biometric data, or internal network configuration are included in this repository.

---

## Engineering Challenges

### 1. Cloud-to-On-Premise Connectivity

The application was hosted in AWS while biometric devices remained inside private networks.

**Challenge:** Establish secure communication without publicly exposing the devices.

**Approach:** Implemented controlled private connectivity using Twingate.

---

### 2. Reliable Attendance Synchronization

Attendance data needed to move reliably between physical devices and the cloud application.

**Challenge:** Network or connectivity failures could interrupt synchronization.

**Approach:** Investigated the complete data path from biometric devices through the private network and secure connectivity layer to the AWS application.

---

### 3. Containerized Production Deployment

Multiple application components needed to run consistently on the production server.

**Challenge:** Managing frontend, backend, reverse proxy, and supporting services.

**Approach:** Used Docker-based deployment with Caddy handling HTTPS and request routing.

---

### 4. Secure Employee Attendance

Employees working away from biometric devices needed an alternative attendance mechanism.

**Challenge:** Preventing fraudulent or proxy attendance.

**Approach:** Combined selfie capture, location validation, and AWS Rekognition-based face verification.

---

## What I Learned

This project provided practical experience beyond development alone, particularly in:

* AWS infrastructure
* Linux administration
* Docker
* Cloud networking
* Hybrid cloud connectivity
* Production troubleshooting
* PostgreSQL infrastructure
* HTTPS and reverse proxies
* AWS managed services
* Security considerations
* Application deployment
* Infrastructure operations

It also provided experience troubleshooting a production environment where application behavior depended on multiple infrastructure and network components.

---

## Repository Scope

This GitHub repository is a **portfolio case study** rather than the source repository for the production application.

The actual application is proprietary company software and is maintained separately in the organization's private development environment.

This repository intentionally contains only:

* Sanitized architecture diagrams
* Infrastructure documentation
* Technology overview
* Engineering challenges
* General deployment concepts

It does **not** contain:

* Proprietary application source code
* Employee data
* Biometric data
* Production database dumps
* Credentials or secrets
* Private keys or certificates
* Internal IP addresses
* Confidential company information

---

## Disclaimer

This project description is provided for professional portfolio purposes. Technical details have been generalized or sanitized to protect company infrastructure, data, and intellectual property.
