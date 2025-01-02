---
title: k8sをきれいな状態にする
---

### ネームスペースから削除

```
kubectl get namespaces

kubectl delete namespace <namespace>
```

### その他、リソースの削除

```
kubectl get pv
kubectl get configmaps -A
kubectl get secrets -A
```

### うまく消えない場合

> Namespace "stuck" as Terminating. How do I remove it?
> 
> [https://stackoverflow.com/questions/52369247](https://stackoverflow.com/questions/52369247)

```
NAMESPACE="your-namespace"

kubectl get namespaces "$NAMESPACE" -o json > tmp.json
jq 'del(.spec.finalizers[] | select(. == "kubernetes"))' tmp.json > tmp_modified.json
kubectl replace --raw "/api/v1/namespaces/$NAMESPACE/finalize" -f tmp_modified.json
```
