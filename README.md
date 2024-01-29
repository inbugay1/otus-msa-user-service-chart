## User Service Helm Chart

### Инструкция по установке

1. Запустить из корневой директории команду

```
helm install user-service .
```

2. Подождать пока поднимутся pods и выполнится job

```
kubectl get pods
```

### Подключение к БД

```
export POSTGRES_PASSWORD=$(kubectl get secret --namespace default user-service-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)
```

```
echo $POSTGRES_PASSWORD
```

```
kubectl run user-service-postgresql-client --rm --tty -i --restart='Never' --namespace default --image docker.io/bitnami/postgresql:16.1.0-debian-11-r24 --env="PGPASSWORD=$POSTGRES_PASSWORD" --command -- psql --host user-service-postgresql -U postgres -d postgres -p 5432
```
Или

```
kubectl port-forward user-service-postgresql-0 54325:5432
```

```
psql --host localhost --port 54325 --username postgres
```

### Удаление

```
helm uninstall user-service
```

```
kubectl delete pvc data-user-service-postgresql-0
```

```
kubectl delete po user-service-postgresql-client
```