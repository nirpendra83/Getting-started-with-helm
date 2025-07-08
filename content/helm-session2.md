

## Session 2: Creating and Hosting Custom Helm Charts

## 🎯 Session Objectives

By the end of this session, you will:
- ✅ Create your own Helm charts from scratch  
- ✅ Customize templates and use values effectively  
- ✅ Understand and use `_helpers.tpl`  
- ✅ Host charts on GitHub as a Helm repository  
- ✅ Apply real-world scenarios (Ingress, ConfigMap, HPA, ServiceAccount)

---

## 🛠️ Step 1: Create a Custom Chart

```bash
helm create nginx-demo
cd nginx-demo
```

This creates the following structure:

```bash
nginx-demo/
├── charts/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── hpa.yaml
│   ├── configmap.yaml
│   ├── serviceaccount.yaml
│   ├── _helpers.tpl
│   ├── notes.txt
│   └── tests/
│       └── test-connection.yaml
```

---

## 📄 `_helpers.tpl`

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

---

## 📦 `values.yaml`

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

serviceAccount:
  create: true
  name: ""
  automount: true
  annotations: {}
```

---

## ⚙️ `deployment.yaml`

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
      serviceAccountName: {{ include "nginx-demo.serviceAccountName" . }}
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

## 🔐 `serviceaccount.yaml`

```yaml
{{- if .Values.serviceAccount.create }}
apiVersion: v1
kind: ServiceAccount
metadata:
  name: {{ include "nginx-demo.serviceAccountName" . }}
  labels:
    {{- include "nginx-demo.labels" . | nindent 4 }}
  {{- with .Values.serviceAccount.annotations }}
  annotations:
    {{- toYaml . | nindent 4 }}
  {{- end }}
automountServiceAccountToken: {{ .Values.serviceAccount.automount }}
{{- end }}
```

---

## 🧪 Test and Notes

### `tests/test-connection.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: {{ include "nginx-demo.fullname" . }}-test
  annotations:
    "helm.sh/hook": test
spec:
  containers:
    - name: busybox
      image: busybox
      command: ["wget"]
      args: ["{{ include "nginx-demo.fullname" . }}:{{ .Values.service.port }}"]
  restartPolicy: Never
```

### `notes.txt`

```gotemplate
NGINX has been deployed successfully!

You can access it with:

  kubectl port-forward svc/{{ include "nginx-demo.fullname" . }} 8080:{{ .Values.service.port }}

Visit http://localhost:8080
```

---

## 🚀 Host on GitHub

```bash
helm package nginx-demo
helm repo index .
# push files to GitHub and enable GitHub Pages
helm repo add myrepo https://<your-username>.github.io/helm-registry
helm repo update
helm install nginx-demo myrepo/nginx-demo
```
