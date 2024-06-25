# GitOps Example IaC repository

## Grant permissions

Add a group `cluster-admins`, and add a user `admin` into this group:

```bash
oc adm groups new cluster-admins
oc adm groups add-users cluster-admins admin
```

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

Create a new ArgoCD instance named `argocd` on OpenShift GitOps operator under `argocd` project.

Notes:
- Can't create route of arogcd server automatically on OpenShift local
- Need manual check applicaitonset/enabled (it is a bug?) to create applicationset controller

Create applicationset for namespace applications:

```bash
oc apply -f argocd/cluster-apps/app-namespace/applicationset.yaml
```

The `appset-namespace` ApplicationSet generates two applicaitons: `namespace-1` and `namespace-2`.

But due to the argocd instance is not a cluster-scoped instance, so it can't create namespaces.

Expected errors: `Failed to load live state: Cluster level Namespace "namespace-1" can not be managed when in namespaced mode`.

