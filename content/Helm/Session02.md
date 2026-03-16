+++
title = "Session 2: Creating and Hosting Custom Helm Charts"
weight = 2
+++

- ✅ Create your own Helm charts from scratch  
- ✅ Customize templates and use values effectively    
- ✅ Host charts on GitHub as a Helm repository  

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

###  NOTES.txt in a Helm Chart

`NOTES.txt` is used to display **post-installation instructions** to users after a Helm chart is installed.

When a user runs:

```bash
helm install myapp ./mychart
```
- Helm automatically prints the content of templates/NOTES.txt.
- This file is typically used to show:

   - Application access URLs
   - Login credentials
   - Verification commands
  - Port-forward instructions

### Examples
```yaml
Thank you for installing {{ .Chart.Name }}!

Your release name is: {{ .Release.Name }}

1. Get the application URL by running these commands:

export POD_NAME=$(kubectl get pods --namespace {{ .Release.Namespace }} -l "app={{ .Chart.Name }}" -o jsonpath="{.items[0].metadata.name}")

kubectl port-forward $POD_NAME 8080:80 --namespace {{ .Release.Namespace }}

2. Access the application at:

http://127.0.0.1:8080

3. To check the status of the deployment:

kubectl get pods -n {{ .Release.Namespace }}

Enjoy using {{ .Chart.Name }}!
```


### Understanding `_helpers.tpl` in Helm Charts

- `_helpers.tpl` is a file used in Helm charts to define **reusable template helpers**.

- These helpers are small template functions that can be reused across multiple template files in the chart.

- The file is usually located at: mychart/templates/_helpers.tpl










### Using GitHub as a Helm Chart Repository

GitHub can be used to host Helm charts by serving them through **GitHub
Pages**.\
Helm requires a repository to contain an `index.yaml` file and packaged
chart files.

A valid Helm repository must contain:

    index.yaml
    chart-name-version.tgz

Example repository structure:

    helm-repo/
    ├── index.yaml
    ├── mychart-0.1.0.tgz

------------------------------------------------------------------------

### Step 1: Create a Helm Chart

Create a new Helm chart.

``` bash
helm create mychart
```

Package the chart.

``` bash
helm package mychart
```

Output:

    mychart-0.1.0.tgz

------------------------------------------------------------------------

### Step 2: Generate Repository Index

Generate the Helm repository index.

``` bash
helm repo index .
```

This creates:

    index.yaml

Directory structure should now look like:

    helm-repo/
    ├── index.yaml
    ├── mychart-0.1.0.tgz

------------------------------------------------------------------------

### Step 3: Push Files to GitHub

Create a new GitHub repository.

Example:

    helm01.repo

Push the following files to the repository:

    index.yaml
    mychart-0.1.0.tgz

Example commands:

``` bash
git init
git add .
git commit -m "Add Helm chart repository"
git branch -M main
git remote add origin https://github.com/<username>/helm01.repo.git
git push -u origin main
```

------------------------------------------------------------------------

### Step 4: Enable GitHub Pages

Go to the repository settings.

    Settings → Pages

Select:

    Source: Deploy from a branch
    Branch: main
    Folder: / (root)

Your Helm repository will be available at:

    https://<github-username>.github.io/<repository-name>/

Example:

    https://nirpendra83.github.io/helm01.repo/

------------------------------------------------------------------------

### Step 5: Add the Helm Repository

Add the GitHub repository to Helm.

``` bash
helm repo add tree https://nirpendra83.github.io/helm01.repo/
```

Update the repository list.

``` bash
helm repo update
```

------------------------------------------------------------------------

### Step 6: Verify Helm Repository

Check available charts.

``` bash
helm search repo tree
```

Example output:

    NAME           CHART VERSION   APP VERSION   DESCRIPTION
    tree/mychart   0.1.0           1.0           Example Helm chart

------------------------------------------------------------------------

### Step 7: Install Chart from GitHub Repository

Install the chart.

``` bash
helm install demo tree/mychart
```

Check deployment:

``` bash
kubectl get pods
```

------------------------------------------------------------------------

### Updating the Helm Repository

Whenever a new chart version is added:

1.  Package the chart again

``` bash
helm package mychart
```

2.  Regenerate the index

``` bash
helm repo index .
```

3.  Commit and push changes

``` bash
git add .
git commit -m "Update Helm chart"
git push
```

------------------------------------------------------------------------

### Summary

Using GitHub as a Helm repository involves the following steps:

1.  Create a Helm chart
2.  Package the chart
3.  Generate `index.yaml`
4.  Push files to GitHub
5.  Enable GitHub Pages
6.  Add the repository using `helm repo add`

Example Helm repository URL:

    https://nirpendra83.github.io/helm01.repo/

Install chart:

``` bash
helm install demo tree/mychart
```

------------------------------------------------------------------------

### Best Practices

-   Use semantic versioning for charts
-   Always update `index.yaml`
-   Keep chart packages (`.tgz`) in the repository
-   Use GitHub Pages instead of GitHub HTML URLs



## ChartMuseum -- Helm Chart Repository Server

ChartMuseum is an **open-source Helm Chart Repository server** that
allows you to store, manage, and distribute Helm charts.

It works similarly to a **Docker Registry**, but instead of container
images it stores **Helm chart packages (.tgz)**.

ChartMuseum allows teams to create **private Helm repositories** for
internal Kubernetes deployments.

------------------------------------------------------------------------

### Why Use ChartMuseum

While Helm charts can be hosted on **GitHub Pages**, ChartMuseum is
preferred for **enterprise environments** because it provides more
control and automation.

### Key Benefits

-   Private Helm chart repository
-   HTTP API for chart uploads
-   Chart version management
-   CI/CD integration
-   Multiple storage backend support

Supported storage backends:

-   Local filesystem
-   AWS S3
-   Google Cloud Storage
-   Azure Blob Storage
-   MinIO

------------------------------------------------------------------------

### ChartMuseum Architecture

Typical workflow:

    Developer
       │
       │ helm push
       ▼
    ChartMuseum Server
       │
       │ stores charts
       ▼
    Storage Backend
       │
       ▼
    Helm Clients

Helm clients retrieve charts using:

    helm repo add
    helm install

------------------------------------------------------------------------

### Installing ChartMuseum (Docker)

The easiest way to run ChartMuseum is using Docker.

``` bash
docker run -d \
-p 8080:8080 \
-e STORAGE=local \
-e STORAGE_LOCAL_ROOTDIR=/charts \
-v $(pwd)/charts:/charts \
ghcr.io/helm/chartmuseum:latest
```

ChartMuseum will start on:

    http://localhost:8080

------------------------------------------------------------------------

### Verify Chart Repository

Open the following URL in a browser:

    http://localhost:8080/index.yaml

Example output:

``` yaml
apiVersion: v1
entries:
  mychart:
    - version: 0.1.0
```

------------------------------------------------------------------------

### Add ChartMuseum Repository to Helm

Add the repository to Helm:

``` bash
helm repo add myrepo http://localhost:8080
```

Update Helm repositories:

``` bash
helm repo update
```

------------------------------------------------------------------------

### Create and Package a Helm Chart

Create a chart:

``` bash
helm create mychart
```

Package the chart:

``` bash
helm package mychart
```

Output:

    mychart-0.1.0.tgz

------------------------------------------------------------------------

### Upload Chart to ChartMuseum

Upload the chart using curl:

``` bash
curl --data-binary "@mychart-0.1.0.tgz" http://localhost:8080/api/charts
```

ChartMuseum will automatically update the repository index.

------------------------------------------------------------------------

### Verify Uploaded Charts

Check available charts:

``` bash
helm search repo myrepo
```

Example output:

    NAME            CHART VERSION   APP VERSION   DESCRIPTION
    myrepo/mychart  0.1.0           1.0           Example chart

------------------------------------------------------------------------

### Install Chart from ChartMuseum

Install the chart:

``` bash
helm install demo myrepo/mychart
```

Verify deployment:

``` bash
kubectl get pods
```

------------------------------------------------------------------------

### List Charts in ChartMuseum

You can check charts using the API:

    http://localhost:8080/api/charts

Example response:

``` json
{
  "mychart": [
    {
      "version": "0.1.0"
    }
  ]
}
```

------------------------------------------------------------------------

### Delete a Chart

Delete a specific chart version:

``` bash
curl -X DELETE http://localhost:8080/api/charts/mychart/0.1.0
```

------------------------------------------------------------------------

### ChartMuseum with CI/CD

ChartMuseum integrates easily with CI/CD tools such as:

-   GitLab CI
-   Jenkins
-   GitHub Actions
-   ArgoCD
-   Tekton

Typical pipeline step:

    helm package mychart
    curl --data-binary "@mychart-0.1.0.tgz" http://chartmuseum/api/charts

------------------------------------------------------------------------

### Best Practices

-   Use semantic versioning for Helm charts
-   Automate chart publishing via CI/CD
-   Use S3 or object storage for production
-   Secure ChartMuseum using authentication

------------------------------------------------------------------------

### Summary

ChartMuseum provides an easy way to host **private Helm repositories**.

Main workflow:

1.  Create Helm chart
2.  Package chart
3.  Upload chart to ChartMuseum
4.  Add repository to Helm
5.  Install charts from the repository

Example commands:

``` bash
helm repo add myrepo http://localhost:8080
helm install demo myrepo/mychart
```
