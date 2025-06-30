+++
title = "Additional"
weight = 10
+++

## 🔍 Preview Manifests Before Installing

To preview the rendered Kubernetes manifests from a Helm chart **without installing it**, use:

```bash
helm template <release-name> <chart-path-or-name>
```

### Example:

```bash
helm template my-nginx bitnami/nginx
```

---

## 🧪 Simulate Installation (Dry Run)

To simulate an install with all validations (including custom values):

```bash
helm install <release-name> <chart-path-or-name> --dry-run --debug
```

### Example:

```bash
helm install my-nginx bitnami/nginx --dry-run --debug
```

---

## 📥 Get Values from Existing Release

To retrieve the custom values used in an existing Helm release:

```bash
helm get values <release-name> -n <namespace>
```

### Example:

```bash
helm get values my-nginx -n default
```

To get **all values** including defaults and overrides (merged output):

```bash
helm get values <release-name> -n <namespace> --all
```

### Example:

```bash
helm get values my-nginx -n default --all
```

---

## 📄 Get Default `values.yaml` from a Helm Chart

To get the default `values.yaml` file from a Helm chart **before installing it**, use:

```bash
helm show values <chart-name>
```

### 📦 Example (from a remote repository):

```bash
helm show values bitnami/nginx
```

### 📁 Example (from a local chart directory):

```bash
helm show values ./mychart
```

This command prints all the default configuration options supported by the chart.

---

### 💾 Save Default Values for Customization

You can redirect the output to a file, edit it, and use it during installation:

```bash
helm show values bitnami/nginx > custom-values.yaml
```

Then install with:

```bash
helm install my-nginx bitnami/nginx --values custom-values.yaml
```

✅ This is a best practice for controlled and repeatable deployments.

## 🔧 Using `-f`, `--values`, and `--set` in Helm

Helm allows customization of charts using:

- `-f` or `--values` to supply a YAML file with configuration overrides.
- `--set` to pass values inline via the command line.

---

### 📁 Using `-f` or `--values` (YAML file)

```bash
helm install my-nginx bitnami/nginx -f custom-values.yaml
```

```bash
helm upgrade my-nginx bitnami/nginx -f dev-values.yaml -n dev
```

You can also supply **multiple files** in order of precedence:

```bash
helm install my-nginx bitnami/nginx -f base.yaml -f prod.yaml
```

> The last file overrides values from the previous ones.

---

### 💡 Using `--set` (Inline values)

Set a single value inline:

```bash
helm install my-nginx bitnami/nginx --set service.type=LoadBalancer
```

Set multiple values inline:

```bash
helm install my-nginx bitnami/nginx \
  --set replicaCount=2 \
  --set image.tag=1.23.0 \
  --set service.type=NodePort
```

---

### 🔄 Combine `--set` and `--values`

You can combine both options. Inline `--set` overrides the values in the file:

```bash
helm install my-nginx bitnami/nginx \
  -f custom-values.yaml \
  --set service.type=LoadBalancer
```

---

### 📝 Set Nested/Array Values with `--set`

For nested keys:

```bash
helm install my-nginx bitnami/nginx \
  --set metrics.enabled=true \
  --set ingress.enabled=true \
  --set ingress.hostname=nginx.example.com
```



---

✅ Use `--values` for maintainable configurations,  
✅ Use `--set` for quick overrides or scripting.
