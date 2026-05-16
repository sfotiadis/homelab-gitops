# Homelab GitOps Platform

> Production-grade workload and platform management for a homelab Kubernetes environment, built on GitOps principles with declarative infrastructure, centralized authentication, observability, and automated operations.

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Flux](https://img.shields.io/badge/Flux_CD-5468FF?style=for-the-badge&logo=flux&logoColor=white)](https://fluxcd.io/)
[![Gateway API](https://img.shields.io/badge/Gateway_API-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://gateway-api.sigs.k8s.io/)

> **Infrastructure Layer**: Underlying compute, networking, and VM provisioning is managed separately via Terraform, Ansible, and Packer in [homelab-automation](https://github.com/sfotiadis/homelab-automation).

## Table of Contents

- [Overview](#overview)
- [Core Plaatform Components](#core-components)
- [Key Features](#key-features)
- [Repository Structure](#repository-structure)
- [Architecture Summary](#architecture-summary)
- [Related Projects](#related-projects)
- [About](#about)
- [Resources](#resources)

## Overview

This repository defines the entire application and platform layer of a GitOps-driven homelab built on Kubernetes via Flux CD.

It implements a cloud-native platform stack including:

- GitOps-based reconciliation of cluster state
- Modern CNI networking with Cilium
- Gateway API-based ingress and traffic management
- Progressive delivery with Flagger
- Centralized identity with Keycloak (OIDC/OAuth2)
- Full observability stack (metrics, logs, time-series, alerts)
- Multi-tenant workload isolation
- Git-encrypted secrets via SOPS


## Core Platform Components

| Component | Purpose |
|----------|--------|
| Flux CD | GitOps continuous delivery |
| Kubernetes | Container orchestration platform |
| Cilium | eBPF-based networking and security |
| Gateway API | Modern ingress abstraction |
| Traefik | Gateway / ingress controller |
| Keycloak | Identity and access management |
| Kyverno | Policy enforcement |
| cert-manager | TLS certificate automation |
| external-dns | DNS automation |
| CloudNativePG | PostgreSQL operator |
| Prometheus | Metrics and alerting |
| Grafana | Dashboards and visualization |
| Loki | Log aggregation |
| MinIO | S3-compatible object storage |
| MetalLB | LoadBalancer for bare metal |
| Flagger | Progressive delivery / canary deployments |
| InfluxDB | Time-series database |
| Telegraf | Metrics collection |


## Key Features

### GitOps-Driven Platform
- Fully declarative cluster state via Git
- Continuous reconciliation using FluxCD
- Encrypted secrets using SOPS + gpg
- Separation of infrastructure and tenant workloads

### Modern Networking Stack
- Cilium as CNI (eBPF-based networking)
- Gateway API for ingress abstraction
- Traefik as gateway controller
- MetalLB for LoadBalancer support
- Automated DNS via external-dns
- TLS everywhere via cert-manager

### Identity & Access Management
- Central SSO via Keycloak
- OAuth2/OIDC integration for services
- Group-based access control

### Policy & Governance
- Kyverno policies for validation and mutation
- Enforced cluster standards and security rules
- Policy reporting and auditability

### Observability Stack
- Prometheus for metrics and alerting
- Grafana for dashboards
- Loki for logs
- InfluxDB for time-series data
- Telegraf for telemetry collection
- Karma for alert UI

### Data & Storage Layer
- PostgreSQL via CloudNativePG
- MinIO for object storage (S3-compatible)
- NFS CSI + local-path provisioner for persistence

### Progressive Delivery
- Flagger for canary deployments
- Metrics-driven rollout strategies via Prometheus


## Repository Structure

- `clusters/` – Flux bootstrap and cluster configuration
- `infrastructure/` – Core platform components (networking, IAM, observability, storage, policies)
- `tenants/` – Application workloads and namespaces


## Architecture Summary

1. **Cluster Bootstrap Layer**
   - Flux installation and Git sources

2. **Infrastructure Layer**
   - Networking (Cilium, MetalLB)
   - Ingress (Gateway API, Traefik)
   - Identity (Keycloak)
   - Observability (Prometheus, Grafana, Loki, InfluxDB)
   - Storage (MinIO, NFS, CNPG)
   - Policy (Kyverno)
   - Delivery (Flagger)

3. **Tenant Layer**
   - Application deployments
   - Namespace isolation
   - GitOps-managed workloads


## Related Projects

- **[homelab-automation](https://github.com/sfotiadis/homelab-automation)**  
  Infrastructure provisioning (Proxmox VE, VMs, Kubernetes bootstrap)

- **homelab-gitops (this repo)**  
  Kubernetes workload and platform management via Flux CD

- **[CruLabs/helm](https://github.com/CruLabs/helm)**  
  Custom helm charts built for this cluster
  


## About

This is my actively running production homelab environment. The setup demonstrates enterprise-grade platform engineering practices applied to a homelab context. Configurations are specific to my environment, but the patterns and architecture can serve as reference for similar setups.


## Resources

- [Flux Documentation](https://fluxcd.io/docs/)
- [Gateway API Docs](https://gateway-api.sigs.k8s.io/)
- [Keycloak Docs](https://www.keycloak.org/documentation)
- [SOPS Guide](https://github.com/mozilla/sops)
- [CloudNativePG](https://cloudnative-pg.io/)
