# ckad udemy


# General notes
- Popular ingress contollers
    ```
    Ambassador: API Gateway based on Envoy with community/commercial support from Datawire
    Voyager: HAProxy based Ingress Controller from AppsCode
    Contour: Envoy based Ingress Controller from Heptio (acquired by VMWare)
    Gloo: Envoy based API Gateway with enterprise support from solo.io
    Citrix: Ingress Controller for MPX, VPX, and CPX ADC products
    F5: Supports F5’s BIG-IP Container Ingress Services
    HAProxy: Community-driven HAProxy Ingress Controller as well as enterprise offering from HAProxy Tech
    Istio: Ingress Gateway for Istio-enabled clusters
    Kong: nginx-based API gateway with community/enterprise options from KongHQ
    NGINX: official Ingress for NGINX and NGINX Plus
    Skipper: HTTP router and reverse proxy from Zalando
    Traefik: HTTP reverse proxy with commercial support from Containous
    ```

# Seetting up Kubernetes / CICD
- [Terragrunt](https://terragrunt.gruntwork.io/) - Terragrunt has the ability to download remote Terraform configurations. The idea is that you define the Terraform code for your infrastructure just once, in a single repo

- [FluxCD](https://fluxcd.io/) - Focuses mostly on CD - canaries, feature flags, and a/b testing. Integrates with helm, aks, jenkins, github, gitlab, bitbucket.

# Section 1

## CKAD Exam
- entire documentation is available to you during the exam. 

# Secction 2

## Core Concepts
- Kubernetes installs
    - api server
        - acts as the front end, user commands, UI, 
    - etcd
        - distributed reliable key-value store used by kubernetes key value store, used by kuber, used to managed the cluster
        - implementing logs within the clusters
    - kublet
        - agent that runs on each node on the cluster. 
    - container runtime
        - is the underlying software that is used to run containers. In our case its docker
    - controller
        - brain behinds orchestration. They are responsible for noticing and responding when nodes, containers, or end points go down. 
    - scheduler
        - distributing work or containers across multiple nodes. looks for newly created containers and assigns them to nodes.

## Pods 
- Pod is the smallest thing you can create in kubernetes pod. 
- scale up create more pod, scale down, delete pods
- multiple conitainers can live in a pod - helper containers 
- `kubectl get pods` 
- `kube run nginx --image nginx`

## Yaml in Kubernetes - Pods
- 

## ReplicaSets
- Replication controller ensures that a "x" number of pods are running at all time. It can restart a single pod and or manage multiple pods. 
```
apiversion: v1
kind: ReplicationController
metadata:
    name: myapp-rc
    labels:
        app: myapp
        type: front-end
spec:
    template:
        metadata:
            name: myapp-pod
            labels:
              app: myapp
              type:front-end
        spec:
            containers:
            - name: nginx-container
              image: nginx
    replicas: 3
    selector:
        matchLables:
            type: front-end   
```

- commands used
    - `kubectl create -f replicaset-definition.yml`
    - `kubectl get replicaset`
    - `kubectl delete replicaset myapp-replicaset`
    - `kubectl replace -f replicaset-definitionl.yml`
    - `kubectl scale -replicas=6 -f replicaset-definition.yml`

## Deployments

- Definition file - almost the same except change `kind: replicaset` to `kind: Deplooyment` 
    - always has apps/v1

- commands used
    - `kubectl get all` gets all objects in kuber
    - `kubectl create -f deployment-definition.yml` - creates a deployment in kuber
    - `kubectl get deployments` - gets the deployments