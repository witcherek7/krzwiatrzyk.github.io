
# Table of content

+ ## [Health Checks](#healthchecks)
+ ## [Resources](#resources)
+ ## [Persisting Data with Volumes](#persvolumes)
+ ## [Pod YAML manifest](#podspec)
+ ## [Service](#service)
+ ## [Service YAML manifest](#servicespec)


# <a name="healthchecks">Health Checks</a>

## Default health check
Kubernetes automatically uses a *process health check* to ensure that container is alive. It simply checks if main process of container is still running. If it is not, then it is restarted.

## Liveness probe
Liveness uses defined method (ex. httpGet) to check if application is ready and responding. If it receives any response beetwen status code 200 and 400 it thinks that container is ready.

Liveness probe is defined under container spec.
```yaml
containers:
    - name: app
      livenessProbe:
        httpGet:
            path: /healthcheck
            port: 8080
        initialDelaySeconds: 5
        timeoutSeconds: 1
        periodSeconds: 10
        failureThreshold: 3
```

## Readiness probe
Readiness probe checks if the container in the service is ready to serve user requests. Containers that fails that test are remove from load balancer so they won't receive any user request. 

```yaml
containers:
  readinessProbe:
    httpGet:
      path: /ready
      port: 8080
    periodSeconds: 2
    initialDelaySeconds: 0
    failureThreshold: 3
    successThreshold: 1
```

Readiness probe is working all the time during Pod lifecycle. It ensures that when Pod is overwhelmed it won't have new requests coming. After it replies for /ready it can handle requests again. Initial delay seconds start after pod is stated as ready (so probably after Liveness probe). Only raedy pods receive traffic.


## Port Check
Kubernetes support *tcpSocket* which check if a TCP port is open. This is useful for non-HTTP apps.

## Exec checks
Execute script in the context of container. This is user defined chech and if script returns zero that check is succesful. Otherwise it fails.


# <a name="resources">Resources</a>
Pod Quota is equal to every container quota. Resources are requested per container, not per Pod.


## Minimum quota
In Kubernetes Pod can request a guaranteed minimium of resources that will be given to him to run its application. The most common resources are CPU and memory. 

```yaml
resources:
  requests:
    cpu: "500m"
    memory: "128Mi"
```

## Maximum quota
It is possible to specify maximum number of resources that container can use in the pod.

```yaml
resource:
  limits:
    cpu: "1000m"
    memory: "256Mi"
```

# <a name="persvolume">Persisting Data with Volumes</a>
Normally containers inside Pod have their own filesystem. This data is ephemeral which means that data is deleted on Pod deletion or containers restart. To avoid loss of data it is possible to add volumes to the pod and mount them to desired containers. 

```yaml
spec:
  volumes:
    - name: "app-data"
      hostPath:
        path: "/var/data"
  containers:
    - name: app
      volumeMounts:
        - mountPath: "/data"
          name: "app-data"
```

# <a name="podsched">Pod scheduling</a>

When user want's to create a pod and submit the manifest file to a sheduler, it find suitable node that have enough resources to run this pod. After pod creation it won't be automatically rescheduled to another node.

# <a name="podspec">Pod YAML manifest</a>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app
spec:
  volumes:
    - name: "app-data"
      hostPath:
        path: "/var/data"
  containers:
    - image: appimage
      name: app
      resources:
        requests:
          cpu: "500m"
          memory: "128Mi"
        limits:
          cpu: "1000m"
          memory: "256Mi"
      ports:
        - containerPort: 8080
          name: http
          protocol: TCP
      livenessProbe:
        httpGet:
            path: /healthcheck
            port: 8080
        initialDelaySeconds: 5
        timeoutSeconds: 1
        periodSeconds: 10
        failureThreshold: 3
      readinessProbe:
        httpGet:
          path: /ready
          port: 8080
        periodSeconds: 2
        initialDelaySeconds: 0
        failureThreshold: 3
        successThreshold: 1
      volumeMounts:
        - mountPath: "/data"
          name: "app-data"
```

# <a name="service">Service</a>

## Pod FQDN
Full DNS name of created service are: <br>
*service_name.namespace_name.svc.cluster.local.*

## Refering to service

To refer to service in namespace only *service_name* can be used. There is no need to use full FQDN. In another namespace *service_name.namespace_name* have to be used. This is useful to share data beetwen different pods.

## Service Types

+ ClusterIP - type of service accessible only within Kubernetes cluster, default type!
+ NodePort - type of service with one external IP and Endpoints attached to it. Incomming traffic will be randomly directed to one of the endpoints (pods).
+ LoadBalancer - types of service built on NodePort that have better balancing flows 

To show endpoints attached to the service
```yaml
kubectl describe endpoints <service_name>
```

# <a name="servicespec>Service YAML manifest
```
apiVersion: v1
kind: Service
metadata:
  name: appservice
spec:
  selector:
    app: app
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
  clusterIP: 172.17.0.50
  type: LoadBalancer
```