+++
title = "Helm Functions"
weight = 5
+++

[Template Functions and Pipelines](https://helm.sh/docs/chart_template_guide/functions_and_pipelines/)

## 🧭 Helm Template Functions Cheat Sheet with Examples

This document provides a comprehensive list of Helm template functions used in Helm charts, with real-world examples using `.Values`, `.Chart`, and `.Release`.

---

## 🔧 Commonly Used Functions

### 🗝️ `default` – Provide a fallback value

```yaml
replicaCount: {{ .Values.replicaCount | default 3 }}
```

> If `.Values.replicaCount` is not set in `values.yaml`, it defaults to `3`.

---

### ❗ `required` – Make a value mandatory

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
data:
  password: {{ required "A password is required!" .Values.password | b64enc }}
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
data:
{{- if .Values.password }}
  password: {{ .Values.password | b64enc | quote }}
{{- else }}
  password: ""
{{- end }}

```
> Will throw an error at install/upgrade if `password` is not provided.

---
## How to render yaml files from new charts 
```yaml
helm template my-nginx01 ./nginx-demo -n helm-test --debug
```
## 🔤 String Functions

### `upper`, `lower`, `title`, `trim`, `replace`

```yaml
metadata:
  name: {{ .Chart.Name | upper }}-{{ .Release.Name | lower }}
  annotations:
    summary: {{ .Values.summary | title | quote }}
```

> Transforms strings:
> - `helm` → `HELM`
> - `release-name` → `release-name`
> - `some text` → `Some Text`

```yaml
annotations:
  clean-name: {{ replace .Chart.Name "-" "_" }}
```

> Replaces hyphens (`-`) with underscores (`_`) in the chart name.  
> Useful for making label-safe names.

---

## 🔢 Number Functions

### `add`, `sub`, `mul`, `div`, `mod`

```yaml
resources:
  limits:
    cpu: {{ mul .Values.cpuBase .Values.cpuFactor }}
```

```yaml
replicaCount: {{ add 2 3 }}  # Output: 5
```

> Perform arithmetic using Helm values.

---

## 📃 List Functions

### `list`, `join`, `first`, `last`, `uniq`, `range`

#### ✅ Example: Looping with `range` and `list`

```yaml
env:
{{- range $env := list "STAGING" "PROD" "DEV" }}
  - name: ENV
    value: {{ $env }}
{{- end }}
```

> Loops over the list and renders each value as an environment variable.

**Rendered output:**

```yaml
env:
  - name: ENV
    value: STAGING
  - name: ENV
    value: PROD
  - name: ENV
    value: DEV
```

---

#### ✅ Example: `join` – Convert a list to comma-separated string

```yaml
labels:
  environments: {{ join "," (list "dev" "staging" "prod") }}
```

> Converts list to string: `dev,staging,prod`.

---

#### ✅ Example: `uniq` – Remove duplicates from list

```yaml
uniq-values: {{ uniq (list 1 2 2 3 1) }}  # Output: [1 2 3]
```

> Removes duplicate values.  
> Use `join` if you need to convert it to string for use in annotations/labels.

---

## 🗂️ Dictionary/Object Functions

### `dict`, `hasKey`, `pluck`, `keys`

```yaml
{{- $config := dict "env" "prod" "region" "us-west" }}
region: {{ $config.region }}
```

```yaml
{{- if hasKey .Values "replicaCount" }}
replicas: {{ .Values.replicaCount }}
{{- end }}
```

> These functions help dynamically access and iterate through key-value pairs.

---

## 🔁 Flow Control

### `if`, `else`, `range`, `with`

```yaml
{{- if .Values.enabled }}
metadata:
  labels:
    enabled: "true"
{{- else }}
metadata:
  labels:
    enabled: "false"
{{- end }}
```

```yaml
{{- range $key, $val := .Values.config }}
  {{ $key }}: {{ $val }}
{{- end }}
```
### in Values.yaml
```yaml
config:
  timeout: 30s
  retries: 5
  logLevel: debug
```
```yaml
{{- with .Values.database }}
host: {{ .host }}
port: {{ .port }}
{{- end }}
```
### in Values.yaml
```yaml
database:
  host: db.example.com
  port: 5432
```
> Conditional logic and scoping for clean, DRY templates.

---

## 🔐 Crypto & Encoding

### `b64enc`, `b64dec`, `sha256sum`

```yaml
data:
  password: {{ .Values.dbPassword | b64enc }}
```

```yaml
hashed: {{ "sensitive" | sha256sum }}
```

> Useful for secrets, hashing, and safe encoding.

---

## 🕒 Date & Time (Limited)

```yaml
annotations:
  generatedAt: {{ now | date "2006-01-02T15:04:05Z07:00" }}
```

> `now` returns current timestamp.  
> Format it using Go time layout (`2006-01-02` is the reference format).

---

## 🧪 Example `values.yaml`

```yaml
replicaCount: 2
enabled: true
summary: "simple chart"
cpuBase: 100
cpuFactor: 2
password: "secret123"
dbPassword: "admin"
envList:
  - name: ENV
    value: PROD
config:
  LOG_LEVEL: debug
  TIMEOUT: 30
database:
  host: db.example.com
  port: 5432
services:
  service1:
    port: 80
  service2:
    port: 443
```

---
