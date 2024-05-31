helm upgrade --namespace="pgo" --create-namespace --atomic --install pgo oci://registry.developers.crunchydata.com/crunchydata/pgo


kubectl apply -f postgres.yaml
kubectl apply -f redis.yaml

kubectl apply -f user-service.yaml
kubectl apply -f banka-service.yaml
kubectl apply -f berza-service.yaml


chmod +x scale-down.sh
./scale-down.sh

kubectl scale deployment banka-4-pgbouncer --replicas=0
kubectl scale deployment redis --replicas=0

kubectl scale deployment user-service --replicas=0
kubectl scale deployment banka-service --replicas=0
kubectl scale deployment berza-servis --replicas=0
kubectl scale deployment frontend --replicas=0

kubectl delete -f postgres.yaml
kubectl delete -f redis.yaml

kubectl delete -f user-service.yaml
kubectl delete -f banka-service.yaml
kubectl delete -f berza-service.yaml