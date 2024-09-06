# Work with Operators in Kubernetes

## Introduction to the Operators

Kubernetes Operators are a design pattern that extends Kubernetes capabilities to manage complex applications and services through the use of controllers.
Operators are specifically designed to automate the management of Kubernetes-based applications, especially those that require complex operations and persistent states.

An Operator is essentially a custom controller that enhances Kubernetes functionality to manage a specific application.
Operators implement operational logic to handle the full lifecycle of the application.

Operators leverage **Custom Resources (CR)** to define new Kubernetes object types that represent components of the application.
To introduce a Custom Resource, a **Custom Resource Definition (CRD)** must be created.
A CRD is a resource that defines the schema for Custom Resources, specifying the required fields and data types.

An Operator acts as a controller that continuously monitors the state of Custom Resources.
It uses the Kubernetes API to read the desired state and compare it to the current state.
If discrepancies are found, the Operator steps in to align the current state with the desired one.

Kuberntes Operators act in what is called a **control loop**.
These are the stages in details:
1. **Observe**: the Operator continually watches and observes the current state of the application and the cluster.
It monitors various resources, such as pods, services, and config maps, as well as any changes in the cluster that might affect the application.
2. **Diff**: after observing the current state, the Operator compares it to the *desired* state of the application, typically specified in the **CR**, based on the **CRD**.
This comparison is often referred to as *diffing*.
3. **Reconcile**: when discrepancies or differences are identified, the Operator takes action to reconcile these differences.
It makes necessary changes to the resources in the cluster, creating, updating, or deleting resources as required to bring the actual state in line with the desired state.

A Kubernetes Operator is typically deployed as a **standard Kubernetes pod**.
This pod runs in a Kubernetes cluster like any other application and can be managed and scaled just like regular workloads.
A Kubernetes Operator is usually created inside a dedicated `namespace`.

The Operator's pod contains the controller logic that *continually watches* for changes in the cluster, especially for newly created or updated CR instances.
While the Operator manages these standard Kubernetes resources, it does so within domain-specific knowledge and automation.
It knows how to manage these resources in a way that is specific to the application it's designed for.

When a Kuberntes controller is installed, new resources are available to the Kubernetes cluster API, these are `CustomResourceDefinition`.
So, even if an Operator lives inside a deidcated namespace, the CR it defines are available at cluster level.

## Introduction to the Operation SDK

The Kubernetes Operator SDK is a set of tools and frameworks designed to streamline the developmente, deployment, and management of Kuberentes Operators.

Threre are three main purposes for the Operator SDK:
- **Accelerated Development**: it provides developers with tools, libraries, and APIs that abstract away complex Kubernetes concepts, allowing quicker development of Operators by leveraging programming languages and frameworks.
- **Consistency and best practices**: it encourages adherence to best practices and consistency in Operator development by providing scaffolding, templates, and guidelines for implementing controllers and managing resources within Kubernetes.
- **Simplified lifecycle management**: Operators built with the SDK automate routine operational tasks, such as deployment, scaling, updates and failure recovery, for complex applications running on Kubernetes.

The Operator SDK let the user set up the entire development cycle for the Operator, by automating some of the most critical aspects:
- **Project generation**: it offers a command-line interface to initialize new Operator projects based on customizable templates, providing a starting point for developing Operators tailored to specific applications or tasks.
- **Multiple framework choice**: it supports the frameworks **Ansible**, **Helm**, and **Go**, allowing developers to choose the most suitable programming language or toolset for building Operators based on their expertise and preferences.
- **Lifecycle management**: it assists in implementing the **Operator Lifecycle Management (OLM)** best practice.
- **Testing and debugging**: it provides built-in tools and utilities for testing Operator login, handling edge cases and debugging.
