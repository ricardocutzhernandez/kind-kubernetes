## Kind

* Creating the cluster with a configuration file
    ```bash
    kind create cluster --config kind-config.yaml --name <CLUSTER_NAME>
    ```
* Getting the configuration of the cluster
    ```
    kubectl cluster-info --context kind-<CLUSTER_NAME>
    ```
* Getting the configuration into a file
    ```
    kind export kubeconfig --kubeconfig ./config.yaml --name <CLUSTER_NAME>
    ```
* Manually setting the context
    ```
    kubectl config --kubeconfig=config.yaml use-context <CLUSTER_NAME>
    ```

### Kubernetes dashboard deployment
* Deploy the dashboard
    ```
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
    ```
* Create a service account
    ```
    kubectl create serviceaccount dashboard -n default
    ```

* Create a cluster role binding
    ```
    kubectl create clusterrolebinding dashboard-admin -n default --clusterrole=cluster-admin --serviceaccount=default:dashboard
    ```
* Get the token:
    ```
    kubectl get secret $(kubectl get serviceaccount dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode
    ```
* Enable the dashboard
    ```
    kubectl proxy
    ```

* Log In into the dashboard web UI
    ```
    http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
    ```

### Pods

* Crear un pod:
    ```
    kubectl run --generator=run-pod/v1 <POD_NAME> --image=<IMAGE>
    ```
* Ver logs de un pod
    ```
    kubectl describe pod <POD_NAME>
    ```
* Eliminar Pods
    ```
    kubectl delete pod <POD_NAME>
    ```
* Manifiesto de un pod
    ```
    kubectl get pod <POD_NAME> -o yaml
    ```
* Ingresar al pod
    ```
    kubectl exec -ti <POD_NAME> -- sh
    ```
* Ver logs de un pod
    ```
    kubectl logs <POD_NAME> -f
    ```
* Port forward en un pod
    ```
    kubectl port-forward <POD_NAME> <POD_PORT>
    ```

### Deployments
* Create a new deployment
    ```
    kubectl apply -f <DEPLOYMENT FILE>
    ```
* Ver el status del deployment
    ```
    kubectl rollout status deployment <NOMBRE_DEPLOYMENT>
    ```
* Mostrar los labesl del deployment
    ```
    kubectl get deployment --show-labels
    ```
* Revisando el historico
    ```
    kubectl rollout history deployment <NOMBRE_DEPLOYMENT>
    ```
* Especificando el change cause
    ```
    kubectl apply -f deployment1.yaml --record
    ```
* Ver los cambios en una revision
    ```
    kubectl rollout history deployment <NOMBRE_DEPLOYMENT> --revision=<NUMERO_REVISION>
    ```
* Realizar un rollback
    ```
    kubectl rollout undo deployment <NOMBRE_DEPLOYMENT> --to-revision=<NUMERO_REVISION>
    ```
### Services
* Obteniendo los servicios
    ```
    kubectl get service <NOMBRE_SERVICIO> --show-labels
    ```
* Describiendo el servicio
    ```
    kubectl describe service <NOMBRE_SERVICIO>
    ```