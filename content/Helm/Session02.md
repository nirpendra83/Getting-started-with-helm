+++
title = "Session 2: Creating and Hosting Custom Helm Charts"
weight = 2
+++

- ✅ Create your own Helm charts from scratch  
- ✅ Customize templates and use values effectively  
- ✅ Understand and use `_helpers.tpl`  
- ✅ Host charts on GitHub as a Helm repository  
- ✅ Apply real-world scenarios (Ingress, ConfigMap, HPA)

---

- [Official Documentation ](https://helm.sh/docs/chart_template_guide/getting_started)
### 🛠️ Step 1: Create a Custom Chart

```bash
helm create nginx-demo
cd nginx-demo
```

This creates the following structure:

```bash
nginx-demo/
├── charts/
├── Chart.yaml
├── templates/
│   ├── deployment.yaml
│   ├── _helpers.tpl
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   └── tests/
└── values.yaml
```

---

## 📄 What is `_helpers.tpl`?

`_helpers.tpl` is a special file used to define reusable template snippets using Go's `template` syntax. It's where you can define helper functions like naming conventions or labels, which you can include in other templates using `{{ include "name" . }}`.

### ✅ Example: `_helpers.tpl`

```gotemplate
{{- define "nginx-demo.name" -}}
nginx
{{- end }}

{{- define "nginx-demo.fullname" -}}
{{ .Release.Name }}-nginx
{{- end }}

{{- define "nginx-demo.labels" -}}
app.kubernetes.io/name: {{ include "nginx-demo.name" . }}
helm.sh/chart: {{ .Chart.Name }}-{{ .Chart.Version }}
app.kubernetes.io/instance: {{ .Release.Name }}
app.kubernetes.io/managed-by: {{ .Release.Service }}
{{- end }}

{{- define "nginx-demo.serviceAccountName" -}}
{{- if .Values.serviceAccount.name }}
{{ .Values.serviceAccount.name }}
{{- else }}
{{ include "nginx-demo.fullname" . }}
{{- end }}
{{- end }}
```

### 📌 Usage in Other Templates

```yaml
metadata:
  name: {{ include "nginx-demo.fullname" . }}
  labels:
    {{- include "nginx-demo.labels" . | nindent 4 }}
```

This avoids duplication and keeps your chart DRY (Don't Repeat Yourself).

---

## 📦 Custom `values.yaml`

```yaml
replicaCount: 2

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: "1.25.2"

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: true
  className: "nginx"
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
  hosts:
    - host: nginx.local
      paths:
        - path: /
          pathType: Prefix

config:
  message: "Welcome to Helm-powered NGINX!"

autoscaling:
  enabled: true
  minReplicas: 2
  maxReplicas: 5
  targetCPUUtilizationPercentage: 70

resources: {}
nodeSelector: {}
tolerations: []
affinity: {}
```

---

## ⚙️ Templates Overview

### 🔸 `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "nginx-demo.fullname" . }}
  labels:
    {{- include "nginx-demo.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ include "nginx-demo.name" . }}
  template:
    metadata:
      labels:
        app: {{ include "nginx-demo.name" . }}
    spec:
      containers:
        - name: nginx
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - containerPort: 80
          env:
            - name: APP_MESSAGE
              valueFrom:
                configMapKeyRef:
                  name: {{ include "nginx-demo.fullname" . }}
                  key: appMessage
```

---

### 🔸 `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ include "nginx-demo.fullname" . }}
spec:
  type: {{ .Values.service.type }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: 80
  selector:
    app: {{ include "nginx-demo.name" . }}
```

---

### 🔸 `configmap.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ include "nginx-demo.fullname" . }}
data:
  appMessage: {{ .Values.config.message | quote }}
```

---

### 🔸 `ingress.yaml`

```yaml
{{- if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ include "nginx-demo.fullname" . }}
  annotations:
    {{- range $key, $value := .Values.ingress.annotations }}
    {{ $key }}: {{ $value | quote }}
    {{- end }}
spec:
  ingressClassName: {{ .Values.ingress.className }}
  rules:
    {{- range .Values.ingress.hosts }}
    - host: {{ .host }}
      http:
        paths:
          {{- range .paths }}
          - path: {{ .path }}
            pathType: {{ .pathType }}
            backend:
              service:
                name: {{ include "nginx-demo.fullname" $ }}
                port:
                  number: {{ $.Values.service.port }}
          {{- end }}
    {{- end }}
{{- end }}
```

---

### 🔸 `hpa.yaml`

```yaml
{{- if .Values.autoscaling.enabled }}
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: {{ include "nginx-demo.fullname" . }}
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: {{ include "nginx-demo.fullname" . }}
  minReplicas: {{ .Values.autoscaling.minReplicas }}
  maxReplicas: {{ .Values.autoscaling.maxReplicas }}
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: {{ .Values.autoscaling.targetCPUUtilizationPercentage }}
{{- end }}
```

---

## 🧪 Run the Chart

```bash
helm install my-nginx ./nginx-demo
helm test my-nginx
```

---

## 🌐 Host Helm Charts on GitHub

### Step 1: Create and Package

```bash
mkdir helm-registry && cd helm-registry
helm create order
helm create delivery

helm package order
helm package delivery
helm repo index .
```

### Step 2: Push to GitHub

```bash
git init
git remote add origin https://github.com/<your-username>/helm-registry.git
git add .
git commit -m "Add Order and Delivery Charts"
git push -u origin main
```

Enable GitHub Pages on the repository (Settings → Pages).

---

### Step 3: Add and Use Repo

```bash
helm repo add teamcharts https://<your-username>.github.io/helm-registry
helm repo update

helm install order-app teamcharts/order
helm install delivery-app teamcharts/delivery
```

---

## ✅ Summary

| Topic                | Description                                                       |
|---------------------|-------------------------------------------------------------------|
| `_helpers.tpl`       | Defines reusable template logic (naming, labels, etc.)           |
| `values.yaml`        | Central place for all configuration values                       |
| Templates            | Refer to values and helpers using Go templating                  |
| Hosting              | GitHub Pages serves `.tgz` charts and `index.yaml` as a repo     |
| Real-World Additions | ConfigMap, Ingress, Autoscaling (HPA) support                    |

---




