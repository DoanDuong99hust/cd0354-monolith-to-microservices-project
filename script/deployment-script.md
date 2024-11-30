# describe pods
kubectl get pods
kubectl describe pod [pod-name]
kubectl logs [pod-name]
kubectl exec --stdin --tty [pod-name] -- [/bin/bash | sh]

# describe services
kubectl get services
kubectl describe services

# describe hpa
kubectl autoscale deployment feed-api --cpu-percent=70 --min=3 --max=5
kubectl describe hpa

# describe deployments
kubectl get deployments

# apply all configmap and secret
kubectl apply -f secret.yaml
kubectl apply -f config-map.yaml

# apply all deployments and services
kubectl apply -f api-feed-deployment.yaml 
kubectl apply -f api-user-deployment.yaml
kubectl apply -f api-feed-service.yaml 
kubectl apply -f api-user-service.yaml 

kubectl apply -f reverse-proxy-deployment.yaml 
kubectl apply -f reverse-proxy-service.yaml 
kubectl apply -f frontend-deployment.yaml 
kubectl apply -f frontend-service.yaml 

# expose services to external internet
kubectl expose deployment frontend --type=LoadBalancer --name=frontend
kubectl expose deployment reverse-proxy --type=LoadBalancer --name=reverse-proxy

# mount file to configmap
kubectl create configmap frontend-app-config --from-file=app.config.json

# mount file to secret
kubectl create secret generic aws-secret --from-file=$HOME/.aws/credentials

volumes:
    - name: aws-credentials
    secret:
        secretName: aws-secret 

volumeMounts:
    - name: aws-credentials
    mountPath: /root/.aws/credentials
    subPath: credentials