# kubernetes-logs-service-account
    ---
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      namespace: default
      name: developer
    rules:
    - apiGroups: ["*"] # "" indicates the core API group
      resources: ["pods","pods/log","deployments"]
      verbs: ["get", "watch", "list"]
    ---
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: developer
      namespace: default
    ---
    apiVersion: rbac.authorization.k8s.io/v1beta1
    kind: ClusterRoleBinding
    metadata:
      name: developer
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: developer
    subjects:
    - kind: ServiceAccount
      name: developer
      namespace: default
# Get the token

    kubectl get secret $(kubectl get serviceaccount developer -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode
# logs script
## nginx-logs.sh ##
    kubectl logs -f deployment/nginx-deployment -n default
