
## 取得
```
git clone https://github.com/mitsuruishihara/k8s-test.git
```

## kindの取得、clusterの作成

```
go install sigs.k8s.io/kind@v0.25.0
cd kind
```

### development
```
kind create cluster --name dev-cluster --config ./kind/kind-dev-config.yaml
kubectl label node dev-cluster-worker node-role=worker
```
### prodiction
```
kind create cluster --name prod-cluster --config ./kind/kind-prod-config.yaml
kubectl label node prod-cluster-worker node-role=worker
kubectl label node prod-cluster-worker2 node-role=worker
```

## デプロイ
### dev
```
helm install dev ./k8s-test -f values-dev.yaml --namespace app --create-namespace
kubectl get all -n app
```

### prod
```
helm install prod ./k8s-test -f values-prod.yaml --namespace app --create-namespace
kubectl get all -n app
```

## アクセス方法
### dev
```
curl localhost:30080
またはブラウザを開いてhttp://localhost:30080にアクセス
```

### prod
```
curl localhost:30081
またはブラウザを開いてhttp://localhost:30081
```

## 更新時
```
# マニフェストまたはvalues.yamlを更新

# dry run
helm upgrade prod ./k8s-test -f values-prod.yaml --dry-run

# 変更をdeploy
helm upgrade prod ./k8s-test -f values-prod.yaml
```