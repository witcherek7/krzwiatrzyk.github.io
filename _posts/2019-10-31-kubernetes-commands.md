



# General commands:

To apply YAML spec file:
```
kubectl apply -f file.yaml
```

To port forward:
```yaml
# Port forwarding is only temporary, it holds as a process in terminal.
# If socat error occurs then use: sudo apt-get install socat -y
kubectl port-forward <pod_name> <port_contanier>:<port_host>
```

To execute command in container:
```yml
kubectl exec <pod_name> <command>
kubectl exec -it <pod_name> <command>

#Example:
kubectl exec -it podzik sh
```

To copy files to and from Containers:
```yml
kubectl cp <pod_name>:/path/to/file.txt ./local/host/file.txt
kubectl cp ./local/host/files.txt <pod_name>:/path/to/pod/file.txt
```

To edit manifest file in kubernetes: (VIM based editor):
```yml
kubectl edit <obj_type> <obj_name>
```

# Monitor commands:
``` yml
kubectl get nodes
kubectl describe <obj_type> <obj_name>
kubectl logs <pod_name>
kubectl get endpoints <service>    ## show pods that can serve requests for this service
```

# Params
Listen for updates:
``` yml
--watch
```