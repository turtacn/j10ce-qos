# j10ce-qos: Enhanced QoS Management for Kubernetes and Beyond

[![Go Version][go-shield]][go-url]
[![License][license-shield]][license-url]
[![Build Status][build-shield]][build-url] [![Go Report Card][goreport-shield]][goreport-url] [![Docs Status][docs-shield]][docs-url] **j10ce-qos** is an open-source project designed to provide an out-of-tree, extensible Quality of Service (QoS) management framework. It aims to enhance workload performance and resource utilization predictability, initially targeting Kubernetes environments, with a vision to support other containerized and non-containerized systems. A core feature of j10ce-qos is the introduction of a **Minimal Reservation** mechanism, ensuring critical workloads receive a guaranteed minimum set of resources.

---

## Table of Contents

* [Introduction](#introduction)
* [Why j10ce-qos?](#why-j10ce-qos)
* [Key Features](#key-features)
* [Architecture](#architecture)
* [Current Status](#current-status)
* [Getting Started](#getting-started)
    * [Prerequisites](#prerequisites)
    * [Building](#building)
    * [Deployment](#deployment)
* [Usage](#usage)
* [Contributing](#contributing)
* [License](#license)

---

## Introduction

In complex, multi-tenant environments like Kubernetes, effective resource management is crucial for ensuring application performance and stability. While Kubernetes provides basic QoS classes (Guaranteed, Burstable, BestEffort), j10ce-qos extends these capabilities by:

1.  **Decoupling QoS logic**: Moving QoS management "out-of-tree" from the core Kubernetes components allows for more rapid innovation, customization, and easier integration with diverse environments.
2.  **Introducing Minimal Reservation**: Providing a stronger guarantee than the standard "Guaranteed" QoS class by ensuring workloads can reserve and access a minimum set of resources even under high contention.
3.  **Extensibility**: Designing a pluggable architecture to support various resource types (CPU, memory, network, disk I/O), different enforcement mechanisms (cgroups, eBPF, tc), and integration with multiple platforms.

j10ce-qos draws inspiration from pioneering projects in the Kubernetes ecosystem such as Katalyst ([CNCF Blog](https://www.cncf.io/blog/2023/12/26/katalyst-a-qos-based-resource-management-system-for-workload-colocation-on-kubernetes/)), Koordinator ([koordinator.sh](https://koordinator.sh/zh-Hans/blog/release-v0.1.0/)), and terway-qos ([GitHub](https://github.com/AliyunContainerService/terway-qos)).

## Why j10ce-qos?

* **Finer-grained control**: Go beyond the standard Kubernetes QoS classes with custom profiles and precise resource guarantees.
* **Stronger isolation for critical workloads**: The Minimal Reservation feature prevents resource starvation for essential services.
* **Improved resource utilization**: By intelligently managing resources and colocation, j10ce-qos can help optimize cluster efficiency.
* **Platform agnostic (Future Goal)**: While starting with Kubernetes, the architecture is designed to be adaptable to other orchestrators or even bare-metal environments.
* **Community-driven and open**: Foster a collaborative environment for developing advanced QoS solutions.

## Key Features

* **Out-of-Tree Kubernetes QoS**: Manages QoS policies independently of Kubernetes releases.
* **Minimal Resource Reservation**: Guarantees a minimum allocation for critical workloads.
* **Multi-Dimensional Resource Management**: Supports CPU, memory, network bandwidth, and disk I/O (planned).
* **Customizable QoS Profiles**: Allows definition of various QoS levels beyond Kubernetes defaults.
* **Pluggable Enforcement Engine**: Supports different resource control backends (e.g., cgroups v1/v2, eBPF for network).
* **Environment Adapters**: Facilitates integration with Kubernetes and potentially other systems.
* **Declarative Configuration**: Manage QoS policies using Kubernetes CRDs or a similar API for other environments.
* **Enhanced Observability**: Provides metrics for monitoring QoS effectiveness and resource allocation (planned).

## Architecture

j10ce-qos employs a layered architecture consisting of a Control Plane and a Data Plane (Node Agent).

* **Control Plane**: Manages QoS policies, reservation requests, and (optionally) interacts with the scheduler. It includes components like the API Server, Policy Manager, and Reservation Manager.
* **Node Agent**: Runs on each managed node, monitoring local resources and enforcing QoS policies by interacting with underlying kernel mechanisms like cgroups and network subsystems.

For a detailed explanation of the architecture, components, and design decisions, please refer to the [Architecture Document](./docs/architecture.md).

## Current Status

**Alpha / Pre-Release**

j10ce-qos is currently in the early stages of development. The core framework and initial feature set are being actively built. Expect breaking changes and rapid evolution. We welcome early adopters and contributors to help shape its future.

## Getting Started

### Prerequisites

* Go 1.20.2 or later
* A Kubernetes cluster (v1.20+) for Kubernetes integration (e.g., kind, Minikube, or a cloud provider's K8s service)
* `kubectl` configured to interact with your cluster
* Docker (for building container images)
* `make` (optional, for using Makefile targets)

### Building

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/turtacn/j10ce-qos.git](https://github.com/turtacn/j10ce-qos.git)
    cd j10ce-qos
    ```

2.  **Build the binaries:**
    *(Makefile targets will be defined later)*
    ```bash
    # Placeholder for build commands, e.g.:
    # make build
    # OR
    # go build ./cmd/j10ce-qos-controller-manager
    # go build ./cmd/j10ce-qos-agent
    ```

3.  **Build container images:**
    *(Dockerfile and Makefile targets will be defined later)*
    ```bash
    # Placeholder for image build commands, e.g.:
    # make docker-build
    ```

### Deployment

Deployment instructions will vary based on the target environment. For Kubernetes:

1.  **Install CRDs:**
    ```bash
    # kubectl apply -f config/crd/bases/j10ce.io_qosprofiles.yaml
    # kubectl apply -f config/crd/bases/j10ce.io_minimalreservationpolicies.yaml
    ```
    *(Actual CRD file paths will be confirmed once defined)*

2.  **Deploy the Controller Manager:**
    ```bash
    # kubectl apply -f deploy/controller-manager.yaml
    ```
    *(Deployment YAML will be provided)*

3.  **Deploy the Node Agent DaemonSet:**
    ```bash
    # kubectl apply -f deploy/agent-daemonset.yaml
    ```
    *(DaemonSet YAML will be provided)*

Detailed deployment guides and configuration options will be available in the `docs` directory as the project matures.

## Usage

Once deployed, you can define `QoSProfile` and `MinimalReservationPolicy` custom resources in your Kubernetes cluster to manage workload QoS.

Refer to the [examples](./examples/) directory and the upcoming API documentation ([docs/api.md](./docs/api.md)) for usage details.

## Contributing

We welcome contributions from the community! Whether it's reporting bugs, suggesting features, improving documentation, or writing code, your help is appreciated.

Please read our [Contributing Guidelines](./CONTRIBUTING.md) (to be created) to get started. You can also check the [open issues](https://github.com/turtacn/j10ce-qos/issues) for areas where you can help.

Key areas for contribution include:
* Development of new resource control plugins (e.g., advanced eBPF programs).
* Integration with other container runtimes or orchestration platforms.
* Enhancements to the scheduling Kube-scheduler extender/plugin.
* Testing and validation on different Kubernetes distributions and workloads.
* Documentation improvements and usage examples.

## License

j10ce-qos is licensed under the [Apache License 2.0][license-url].

---

* [go-shield]: https://img.shields.io/badge/go-1.20.2+-blue.svg
* [go-url]: https://golang.org/dl/
* [license-shield]: https://img.shields.io/badge/License-Apache%202.0-blue.svg
* [license-url]: https://www.apache.org/licenses/LICENSE-2.0
* [build-shield]: https://img.shields.io/github/actions/workflow/status/turtacn/j10ce-qos/main.yml?branch=main&style=flat-square 
* [build-url]: https://github.com/turtacn/j10ce-qos/actions 
* [goreport-shield]: https://goreportcard.com/badge/github.com/turtacn/j10ce-qos
* [goreport-url]: https://goreportcard.com/report/github.com/turtacn/j10ce-qos
* [docs-shield]: https://img.shields.io/badge/docs-passing-brightgreen 
* [docs-url]: https://github.com/turtacn/j10ce-qos/tree/main/docs 