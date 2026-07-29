# Homelab GitOps Platform

> Production-grade workload and platform management for a homelab Kubernetes environment, built on GitOps principles with declarative infrastructure, centralized authentication, observability, and automated operations.

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Flux](https://img.shields.io/badge/Flux_CD-5468FF?style=for-the-badge&logo=flux&logoColor=white)](https://fluxcd.io/)
[![Gateway API](https://img.shields.io/badge/Gateway_API-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://gateway-api.sigs.k8s.io/)

> **Infrastructure Layer**: Underlying compute, networking, and VM provisioning is managed separately via Terraform, Ansible, and Packer in [homelab-automation](https://github.com/sfotiadis/homelab-automation).

## Table of Contents

- [Overview](#overview)
- [Core Plaatform Components](#core-components)
- [Repository Structure](#repository-structure)
- [Related Projects](#related-projects)
- [About](#about)
- [Resources](#resources)

## Overview

This repository defines the entire application and platform layer of a GitOps-driven homelab built on Kubernetes via Flux CD.

It implements a cloud-native platform stack including:

- GitOps-based reconciliation of cluster state
- Declarative platform and workload management
- Modern eBPF-based networking with Cilium
- Gateway API-based ingress and traffic management
- LoadBalancer services with MetalLB
- Automated DNS and TLS certificate management
- Progressive delivery with Flagger
- Centralized identity and access management with Keycloak
- Kubernetes policy enforcement with Kyverno
- Full observability stack for metrics, logs, dashboards, and alerts
- Distributed storage with Rook-Ceph and Ceph CSI
- PostgreSQL and TimescaleDB workloads managed through CloudNativePG
- Multi-tenant workload isolation and network policies
- Git-encrypted secrets using SOPS
- Self-hosted Git and container registry infrastructure
- Messaging and in-memory data services
- Infrastructure and IoT telemetry collection

## Core Platform Components

| Component | Purpose |
|----------|--------|
| Flux CD | GitOps continuous delivery and cluster reconciliation |
| Kubernetes | Container orchestration platform |
| Cilium | eBPF-based networking and network security |
| Gateway API | Modern ingress and traffic management API |
| Traefik | Gateway API controller |
| MetalLB | LoadBalancer implementation |
| cert-manager | TLS certificate automation |
| external-dns | Automated DNS record management |
| Keycloak | Centralized identity and access management |
| Kyverno | Kubernetes policy enforcement and validation |
| Policy Reporter | Policy reporting and visibility |
| CloudNativePG | PostgreSQL operator and database management |
| Rook-Ceph | Distributed storage and S3-compatible object storage |
| Ceph CSI Drivers | Ceph-backed persistent storage integration |
| Local Path Provisioner | Local persistent volume provisioning |
| Prometheus | Metrics collection and alerting |
| Grafana | Metrics visualization and dashboards |
| Loki | Log aggregation |
| Promtail | Log collection for Loki |
| Telegraf | Infrastructure and IoT telemetry collection |
| Flagger | Progressive delivery and canary deployments |
| RabbitMQ | Message broker |
| Valkey | In-memory data store |
| Harbor | Container image registry |
| Gitea | Self-hosted Git service |
| Homer | Homelab service dashboard |
| Karma | Alert dashboard |

## Repository Structure

- `clusters/` – Flux bootstrap and cluster configuration
- `infrastructure/` – Core platform components (networking, IAM, observability, storage, policies)
- `tenants/` – Application workloads and namespaces

## Related Projects

- **[homelab-automation](https://github.com/sfotiadis/homelab-automation)**  
  Infrastructure provisioning (Proxmox VE, VMs, Kubernetes bootstrap)

- **homelab-gitops (this repo)**  
  Kubernetes workload and platform management via Flux CD

- **[crulabs/charts](https://github.com/crulabs/charts)**  
  Custom helm charts built for this cluster


## About

This is my actively running production homelab environment. The setup demonstrates enterprise-grade platform engineering practices applied to a homelab context. Configurations are specific to my environment, but the patterns and architecture can serve as reference for similar setups.


## Resources

- [Flux Documentation](https://fluxcd.io/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Cilium Documentation](https://docs.cilium.io/)
- [Gateway API Documentation](https://gateway-api.sigs.k8s.io/)
- [Traefik Documentation](https://doc.traefik.io/traefik/)
- [Keycloak Documentation](https://www.keycloak.org/documentation)
- [Kyverno Documentation](https://kyverno.io/docs/)
- [SOPS Guide](https://github.com/getsops/sops)
- [CloudNativePG Documentation](https://cloudnative-pg.io/)
- [Rook Documentation](https://rook.io/docs/rook/latest/)
- [Ceph Documentation](https://docs.ceph.com/)
- [Flagger Documentation](https://docs.flagger.app/)
