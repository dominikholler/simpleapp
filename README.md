# simpleapp

    buildah build -t simpleapp .
    podman run -p 5000:5000 simpleapp

namespace=simpleapp
oc new-project $namespace
oc label namespace $namespace argocd.argoproj.io/managed-by=openshift-gitops


project: simpleapp
source:
  repoURL: https://github.com/dominikholler/simpleapp.git
  path: vms
  targetRevision: HEAD
  directory:
    recurse: true
    jsonnet: {}
destination:
  server: https://kubernetes.default.svc
  namespace: simpleapp
syncPolicy:
  automated:
    prune: false
  syncOptions:
    - CreateNamespace=true
