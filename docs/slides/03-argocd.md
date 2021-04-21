# ArgoCD
* Deklarativ setup för kubernetes manifest
* Direkt versionshantering för Kubernetes state


## Kubernetes
* Kubernetes själv är deklarativt
* ArgoCD expanderar detta till Kubernetes konfigurationen



![argo](../images/argocd-ui.gif)



## Flöde
<img class="r-stretch" src="../images/setup.svg">



## Audit log
<img class="r-stretch" src="../images/git-audit.png">



## Applikation i ArgoCD
<pre><code data-line-numbers="0-80|11-15|16-18">apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: argocd
  namespace: argocd
  finalizers:
    - resources-finalizer.argocd.argoproj.io
spec:
  project: default

  source:
    path: charts/argo-cd
    repoURL: https://ilgit01.domain.se/frla01/gitops.git
    targetRevision: HEAD

  destination:
    server: "https://kubernetes.default.svc"
    namespace: argocd

  syncPolicy:
    automated:
      prune: true
      selfHeal: true
</code></pre>



## Applikation till flera kluster
<pre><code data-line-numbers="0-80|7-13|14-80">apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: cert-manager
  namespace: argocd
spec:
  generators: 
  - list:
      elements:
      - cluster: mgm
        url: https://kubernetes.default.svc
      - cluster: test
        url: https://ilrancherha01.domain.se/k8s/clusters/c-9gg4s
  template:
    metadata:
      name: '{{"{{cluster}}"}}-cert-manager'
      annotations:
        argocd.argoproj.io/sync-wave: "-1"
    spec:
      project: default
      source:
        path: charts/cert-manager
        repoURL: https://ilgit01.domain.se/frla01/gitops.git
        targetRevision: HEAD
        helm:
          valueFiles:
          - values.yaml
          - 'values-{{"{{cluster}}"}}.yaml'
      destination:
        server: '{{"{{url}}"}}'
        namespace: cert-manager
</code></pre>