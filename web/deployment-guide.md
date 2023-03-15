1. test django

```python
python manage.py test
```

2. Build container

```bash
docker build \
    -tregistry.digitalocean.com/chrisocean/django-k8s-web:latest 
    -tregistry.digitalocean.com/chrisocean/django-k8s-web:v1 .
```

3. Push this container to digitalocan registry

```bash
docker push registry.digitalocean.com/chrisocean/django-k8s-web --all-tags
```

4. update secrets

```bash
kubectl delete secret django-k8s-web-prod-env
kubectl create generi django-k8s-web-prod-env --from-env-file=web/.env.prod
```

5. update deplyement

```bash
kubectl apply -f k8s/apps/django-k8s-web.yaml
```

kubectl exec -it $SINGLE_POD_NAME -kubectl exec -it $SINGLE_POD_NAME -- bash /app/migrate.sh- bash /app/migrate.sh
6. wait for Rollout to finish

```bash
kubectl rollout status deploy/django-k8s-web-deployment

```

7. Migrate database

```bash
export SINGLE_POD_NAME=$(kubectl get pod -l app=django-k8s-web-deployment -o jsonpath="{.items[0].metadata.name}")
```

or

```bash
export SINGLE_POD_NAME=$(kubectl get pod -l=app=django-k8s-web-deployment -o NAME | tail -n 1)
```

```bash
kubectl exec -it $SINGLE_POD_NAME -- bash /app/migrate.sh
```
