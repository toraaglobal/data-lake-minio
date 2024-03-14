## Building a Data Lake Using MinIO in a Kubernetes Cluster

---

### Setting up a Kubernetes Cluster

Start by setting up a Kubernetes cluster using a platform like Minikube, kubeadm, or a managed Kubernetes service.

- download and docker desktop : https://www.docker.com/products/docker-desktop/

- click on setting on the upper right conner to expand

- click on kubernates and enable it

### Deploying MinIO

https://min.io/

https://min.io/docs/minio/kubernetes/upstream/operations/installation.html


Deploy MinIO on Kubernetes using Helm charts or YAML manifests. Configure MinIO to use persistent volumes for data storage.

To check whether the kube-controller-manager specifies the cluster signing key and certificate files, use the following command:

```
echo CLUSTERNAME=trino

kubectl get pod kube-controller-manager-$CLUSTERNAME-control-plane \
  -n kube-system -o yaml

```

Verify that th eoutput contains cluster signing key and certificate files

```
spec:
  containers:
  - command:
    - kube-controller-manager
    - --allocate-node-cidrs=true
    - --authentication-kubeconfig=/etc/kubernetes/controller-manager.conf
    - --authorization-kubeconfig=/etc/kubernetes/controller-manager.conf
    - --bind-address=127.0.0.1
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --cluster-cidr=10.244.0.0/16
    - --cluster-name=trino
    - --cluster-signing-cert-file=/etc/kubernetes/pki/ca.crt
    - --cluster-signing-key-file=/etc/kubernetes/pki/ca.key
```


- Install the MinIO Kubernetes Plugin

Krew is a kubectl plugin manager developed by the Kubernetes SIG CLI group. See the krew installation documentation for specific instructions. You can use the Krew plugin for Linux, MacOS, and Windows operating systems.

krew installation instruction: https://krew.sigs.k8s.io/docs/user-guide/setup/install/


Now Install minio using krew 

```
kubectl krew update
kubectl krew install minio

```


- Initialize the MinIO Kubernetes Operator 

```
kubectl minio init
```


kubectl minio proxy -n minio-operator 

- Validate the Operator Installation

```
kubectl get all --namespace minio-operator
```


Use the following command to retrieve the JWT token necessary for logging into the Operator Console:

```
kubectl get secret/console-sa-secret -n minio-operator -o json | jq -r '.data.token' | base64 -d

```




### Configuring MinIO

Customize MinIO configuration parameters, such as access keys, secret keys, and bucket policies, according to your requirements.

Access Key:  FECMN6S7su0dyNLq
Secret Key:  3Kt12YoprqtvnH7gMRr7BPtfx8OW8AgS


### Integrating with Applications

Integrate MinIO with data processing and analytics applications using the S3 API or native SDKs.



## Monitoring and Management

Use Kubernetes monitoring tools like Prometheus and Grafana to monitor MinIO performance and resource utilization. Implement backup and disaster recovery strategies to ensure data integrity and availability.
