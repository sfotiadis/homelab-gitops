# Homelab GitOps Platform

> Production Kubernetes workload management for my homelab cluster, powered by GitOps principles with centralized authentication, observability, and automated operations.

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Flux](https://img.shields.io/badge/Flux_CD-5468FF?style=for-the-badge&logo=flux&logoColor=white)](https://fluxcd.io/)
[![Gateway API](https://img.shields.io/badge/Gateway_API-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://gateway-api.sigs.k8s.io/)
[![Keycloak](https://img.shields.io/badge/Keycloak-008aaa?style=for-the-badge&logo=keycloak&logoColor=white)](https://www.keycloak.org/)

> **Infrastructure Setup**: The underlying infrastructure (Proxmox VE, VMs, networking) is provisioned using Terraform, Ansible, and Packer in [homelab-automation](https://github.com/sfotiadis/homelab-automation).

## Table of Contents

- [Overview](#overview)
- [Core Components](#core-components)
- [Key Features](#key-features)
- [Repository Structure](#repository-structure)
- [Related Projects](#related-projects)
- [About](#about)
- [Resources](#resources)

## Overview

This repository manages application workloads for my productive homelab Kubernetes cluster using GitOps with Flux CD. It implements modern cloud-native patterns including:

- **Gateway API** for next-generation ingress management
- **Centralized Identity Management** with Keycloak OAuth2/OIDC SSO
- **GitOps Workflows** with Flux CD for declarative operations
- **Secrets Management** using SOPS encryption
- **Full Observability Stack** with Prometheus, Grafana, and Loki
- **Automated DNS** with external-dns and RFC2136
- **Multi-tenant Architecture** with namespace isolation

### Core Components

| Component | Version | Purpose |
|-----------|---------|---------|
| **Flux CD** | v2.7.5 | GitOps continuous delivery |
| **Gateway API** | v1.2.1 | Modern ingress and traffic management |
| **Traefik** | v32.1.0 | Gateway controller implementation |
| **Keycloak** | v25.0.0 | Identity and access management |
| **cert-manager** | v1.18.2 | Automated certificate management |
| **external-dns** | v0.19.0 | Automatic DNS record management |
| **CloudNativePG** | Latest | PostgreSQL operator for databases |
| **Prometheus Stack** | Latest | Metrics collection and alerting |
| **Grafana** | Latest | Observability and dashboards |
| **Loki** | Latest | Log aggregation |
| **MinIO** | v5.4.0 | S3-compatible object storage |

## Key Features

### GitOps-Driven Operations
- **Declarative Infrastructure**: All cluster state managed via Git
- **Automated Reconciliation**: Flux continuously syncs cluster state
- **Secrets Encryption**: SOPS with age encryption for secure secrets
- **Multi-Environment Support**: Infrastructure and tenant separation

### Modern Networking
- **Gateway API**: Next-generation Kubernetes ingress (replacing Ingress resources)
- **Automated DNS**: external-dns creates DNS records for HTTPRoutes
- **TLS Everywhere**: Custom CA with wildcard certificates (*.crulabs.intern)
- **Service Mesh Ready**: Gateway API foundation for future mesh integration

### Centralized Authentication
- **Single Sign-On**: Keycloak OAuth2/OIDC for all services
- **Group-Based Access Control**: Fine-grained permissions via Keycloak groups
- **Grafana Integration**: Role mapping (admin/editor) via OAuth
- **MinIO Integration**: Policy-based access (consoleAdmin/readwrite)

### Observability & Monitoring
- **Metrics**: Prometheus with ServiceMonitors
- **Visualization**: Grafana dashboards with SSO
- **Logs**: Loki log aggregation with Promtail
- **Alerting**: Prometheus Alertmanager integration

### Data Management
- **Database as a Service**: CloudNativePG for PostgreSQL clusters
- **Persistent Storage**: NFS CSI driver and local-path provisioner
- **Object Storage**: MinIO with SSO and backup capabilities
- **Automated Backups**: Integrated backup solutions

## Repository Structure

```
.
├── clusters/
│   └── crulabs-k8s/             # Cluster-specific configurations
│       ├── infrastructure.yaml  # Infrastructure components
│       └── tenants.yaml         # Tenant workloads
│
├── infrastructure/              # Platform infrastructure
│   ├── cert-manager/            # Certificate management
│   ├── cloudnative-pg/          # PostgreSQL operator
│   ├── cluster-issuer/          # Custom CA configuration
│   ├── external-dns/            # DNS automation
│   ├── gateway-api/             # Gateway and HTTPRoutes
│   ├── keycloak/                # Identity provider
│   ├── kube-prometheus-stack/   # Monitoring stack
│   ├── local-path-provisioner/  # Local storage
│   ├── loki/                    # Log aggregation
│   ├── minio/                   # Object storage
│   └── nfs-csi-driver/          # NFS storage
│
├── tenants/                     # Application workloads
│   ├── ca-download/             # CA certificate distribution
│   └── shelly-monitoring/       # IoT device monitoring
│
└── charts/                      # Custom Helm charts
    └── cnpg-cluster/            # CloudNativePG cluster chart
```

## Related Projects

- **[homelab-automation](https://github.com/sfotiadis/homelab-automation)** - Infrastructure provisioning with Terraform, Ansible, and Packer on Proxmox VE (VMs, Kubernetes cluster, networking)
- **homelab-gitops** (this repo) - Kubernetes workload and application management with Flux CD

## About

This is my actively running production homelab environment. The setup demonstrates enterprise-grade platform engineering practices applied to a homelab context. Configurations are specific to my environment, but the patterns and architecture can serve as reference for similar setups.

## Resources

- [Flux Documentation](https://fluxcd.io/docs/)
- [Gateway API Docs](https://gateway-api.sigs.k8s.io/)
- [Keycloak Docs](https://www.keycloak.org/documentation)
- [SOPS Guide](https://github.com/mozilla/sops)
- [CloudNativePG](https://cloudnative-pg.io/)
