```bash
# deploy applocation
$ kubectl apply -f deploy.yaml

# check node
$ kubectl get pods my-app-deployment-586487cbf4-g24gx -oyaml | grep nodeName

# port-forward openebs rest api service 
$ kubectl port-forward -n openebs svc/openebs-api-rest 8080

# get mayastor volumes
$ kubectl mayastor get volumes -r https://localhost:8080 # the target node for volume is the same node

# insert some data
$ kubectl exec -it my-app-deployment-586487cbf4-g24gx -- sh -c 'echo "testing" > /sample/data/test.txt'

# check the data
$ kubectl exec -it my-app-deployment-586487cbf4-g24gx -- sh -c 'cat /sample/data/test.txt'

# now cordon the node and delete the pod
$ kubectl cordon ishtiaq-1
$ kubectl delete pod my-app-deployment-586487cbf4-g24gx

# new pod will be created in a new node and check the target node of the mayastor volume
$ kubectl get pods my-app-deployment-586487cbf4-q9dkd -oyaml | grep nodeName
$ kubectl mayastor get volumes -r https://localhost:8080

# check the data
$ kubectl exec -it my-app-deployment-586487cbf4-q9dkd -- sh -c 'cat /sample/data/test.txt'
# if the data is found then it implies that the replication is working
```