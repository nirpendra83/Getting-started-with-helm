+++
title = "Helm Table of Contents"
weight = 1
+++

## Table of Contents

## 📌 Introduction to Helm

- What is Helm?
- Why use Helm for Kubernetes application deployment?
- Key components: Charts, Repositories, Releases

---

## 📚 Exploring the Official Helm Documentation

- Navigating [helm.sh/docs](https://helm.sh/docs/)
- Installation and prerequisites
- Understanding the Helm CLI and commands
- Helm 3 vs Helm 2 (brief overview)

---

## 🧩 Working with Existing Helm Charts

### 🔍 Discovering and Using Helm Repositories

- Adding public and private chart repositories
- Searching for charts (`helm search hub` vs `helm search repo`)
- Repository management commands

### 🚀 Deploying a Helm Release

- Installing a chart as a release
- Setting a namespace for the deployment
- Dry run and debug before actual deployment

### ⚙️ Customizing Charts with Values

- Understanding `values.yaml`
- Overriding values using:
  - Custom `values.yaml` files
  - `--set` CLI option
  - `--set-file` and `--set-string`

### 🔁 Managing Helm Releases

- Upgrading a release with updated values or chart versions
- Rolling back to a previous release version
- Viewing and diffing historical release versions
- Uninstalling a release cleanly

### 🌐 Multi-Environment Helm Usage

- Structuring values for different environments (dev, staging, prod)
- Folder-based environment separation
- Using `--values` with multiple files for environment-specific configs

---

## 🛠️ Creating Your Own Helm Charts

### ⚡ Helm Chart Quick Start

- `helm create mychart`
- Directory structure explained
- Modifying the default template

### 🧬 Helm Template Engine and Syntax

- How Helm renders templates
- Using `{{ .Values }}`, `{{ .Chart }}`, `{{ .Release }}` objects

### 🏗 Built-in Objects and Functions

- Overview of available Helm objects
- Useful template functions (`include`, `required`, `lookup`, `toYaml`, etc.)
- Best practices in chart templating

### 🧪 Testing and Validating Charts

- `helm lint` and template validation
- Using `helm template` for local rendering
- Unit testing templates with tools like `helm-unittest`

---

## 🧰 Troubleshooting and Tips

- Debugging with `--debug` and `--dry-run`
- Common errors and how to fix them
- Checking logs and Helm release history
- Understanding exit codes and command output

---

