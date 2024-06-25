# GitOps Example IaC repository


## ArgoCD Application

### Spring Petclic

```bash
oc apply -f apps/spring-petclinic
```

## ArgoCD ApplicationSet and AppProject

### bgd

```bash
oc apply -f apps/bgd
```

## Helm 

### Deploy a Helm Chart from a Helm repository

```bash
oc apply -f apps/todolist
```

### Deploy a Helm Chart from a Git repository

```bash
oc apply -f apps/todolist-git
```

### Deploy a Helm Chart from a Helm repository with external Helm values from a Git repository

```bash
oc apply -f apps/todolist-multi-sources-dev
```

## App of Apps

Deply an ArgoCD application contains a few child apps.

```bash
oc apply -f app-of-apps/application.yaml
```


## Custom ArgoCD instance

Create a namespace to install a new ArgoCD instance:
```bash
oc new-project argocd
```

Create a new ArgoCD instnace on OpenShift GitOps operator under `argocd` project.
