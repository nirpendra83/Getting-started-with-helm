+++
title = "Introduction to Helm"
type = "Home"
+++

![ocp01](/images/helm.png)

## Table of Contents

- [Overview: What is Helm?](#overview-what-is-helm)
- [Official Helm Documentation](https://helm.sh)
- Working with Existing Helm Charts
  - Adding and Managing Repositories
  - Installing a Helm Release
  - Using and Customizing `values.yaml`
  - Overriding Values with `--set`
  - Upgrading an Existing Release
  - Rolling Back to a Previous Release
  - Viewing Current Values of a Release
  - Managing Multiple Environments
  - Debugging and Dry Runs
- Creating Helm Charts
  - [Quick Start Guide](https://helm.sh/docs/intro/quickstart/)
  - [Template Syntax & Structure](https://helm.sh/docs/chart_template_guide/getting_started/)
  - [Built-in Template Objects](https://helm.sh/docs/chart_template_guide/builtin_objects/)

## Overview: What is Helm?

Helm is the package manager for Kubernetes. It enables developers and operators to package, configure, deploy, and manage applications on Kubernetes clusters using Helm charts—predefined templates that streamline complex deployments.

## Prerequisites

Before you begin, ensure you have:

- A basic understanding of Kubernetes concepts  
- Familiarity with Linux command-line operations

## Required Tools

To follow along and perform hands-on tasks, you'll need:

- Visual Studio Code (VSCode) or any preferred text editor  
- The `kubectl` CLI to interact with Kubernetes  
- The Helm CLI (`helm`) installed and configured

## Hands-On Labs

This guide includes practical labs where you'll learn how to:

- Deploy applications using publicly available Helm charts  
- Customize deployments by editing chart values  
- Create and deploy your own Helm charts  
- Use Helm’s core features such as install, upgrade, rollback, and templating
