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

If use default `openshift-gitops` argocd instance, it can create namespaces normally.

```bash
oc apply -f argocd/cluster-apps/app-namespace/applicationset-v2.yaml
```

If `argocd` argocd instance wants to deloy applicationt to the namespaces managed by `openshift-gitops` argocd instance, need add label `argocd.argoproj.io/managed-by`.

Example:
```bash
oc label namespace namespace-v2-1 argocd.argoproj.io/managed-by=argocd --overwrite=true
```

Otherwise has the error: `Failed to load live state: Namespace "namespace-v2-1" for Deployment "spring-petclinic" is not managed`.

References:
- <https://github.com/argoproj-labs/argocd-operator/issues/665>
- <https://developers.redhat.com/articles/2021/08/03/managing-gitops-control-planes-secure-gitops-practices>
- <https://docs.openshift.com/gitops/1.12/declarative_clusterconfig/configuring-an-openshift-cluster-by-deploying-an-application-with-cluster-configurations.html#using-argo-cd-instance-to-manage-cluster-scoped-resources_configuring-an-openshift-cluster-by-deploying-an-application-with-cluster-configurations>

