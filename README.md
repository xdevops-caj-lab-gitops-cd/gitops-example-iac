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