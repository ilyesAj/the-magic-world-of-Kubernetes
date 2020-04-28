# Configuration

In this markdown we will discuss how we can configure your application within kubernetes 

## Environnement variables

We can set configutions by passing them as a global vars directly in the pod : 

````yml
apiVersion: v1
kind: Pod
metadata:
  name: envar-demo
  labels:
    purpose: demonstrate-envars
spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env:
    - name: city
      value: "vélizy"
    - name: state
      value: "ile-de-france"
````

the problem in this approch is the workload portability : To change a configuration of a pod you have to access to the pod definition and modify it . Using ConfigMap guaranties keeping containerized applications portable.

## ConfigMap

Configmap data is stored “outside” of the cluster **(in etcd)**
They are insanely versatile. You can inject them as environment variables at runtime, as a command arg, or as a file or folder via a volume mount.

- **declarative way:**
Write config map manually :  

````yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: manifest-example
data:
  city: vélizy
  state: ile-de-france
````

- **imperative way :**
  - **As a string :** `kubectl create configmap literal-example --from-literal="city=vélizy" --from-literal=state=ile-de-france`
  - **From a file(s)** : `kubectl create configmap file-example --from-file=cm/city --from-file=cm/state`
  - **From a folder:** , kubectl identifies files whose basename is a valid key in the directory and packages each of those files into the new ConfigMap. Any directory entries except regular files are ignored`kubectl create configmap dir-example --from-file=cm`

the ``configMap`` is then injected into the pod definition as environment vars or as a volume :

- environment vars:

````yml
...
spec:
  Containers:
  - name: envar-demo-container
    ...
    envFrom:
      - configMapRef:
          name: manifest-example
...

````

- Volume mount :

````yml
spec:
  Containers:
  - name: envar-demo-container
    ...
    volumeMounts:
    - name: appConfigVol
      mountPath: /etc/config
  volumes:
  - name: appConfigVol
    ConfigMap:
      name: manifest-example
````

## Secret

Based on ``configMap``, this resource is essentially used for storing password and credentials . the only difference between `secret` and `configMap` is that `secret` Stores data as base64 encoded content and can be encrypted at rest within etcd (if configured!).  
**🏴‍☠️ encoding a secret is not securing it !!**
💁 refer to this [conference](https://www.youtube.com/watch?v=f4Ru6CPG1z4)
>**why we use base64:**  
This allows you to encode arbitrary bytes to bytes which are known to be safe to send without getting corrupted (ASCII alphanumeric characters and a couple of symbols).

example :

````yml
apiVersion: v1
kind: Secret
metadata:
  name: manifest-data
type: opaque
data:
  username: aWx5ZXMK
  password: cGFzc3dvcmQK
````

- `type:`
  - `Opaque` arbitrary data(unstructured) .this is the default value
  - `kubernetes.io/service-account-token` : Kubernetes auth token
  - `kubernetes.io/dockercfg`: Docker registry auth
  - `kubernetes.io/dockerconfigjson`: Latest Docker registry auth
  - `tls`: Create a TLS secret from the given public/private key pair.
  The public key certificate must be .PEM encoded and match the given private key.  
  a special syntax will be used in this case. 

- `data:` Contains key-value pairs of base64 encoded content (if the data inserted in plain text , k8s will automatically encode it with `base64`

>create a secret for the docker registry: 

````sh
kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
````

> create a secret for TLS :
>
>````sh
>kubectl create secret tls tls-secret --cert=path/to/tls.cert --key=path/to/tls.key
>````

### decrypt a secret

````sh
kubectl get secrets db-secret -o yaml
# use `echo 'encoded_password' | base64 --decode` to view in plain text
````

### injecting secrets

- environment vars:

````yml
...
spec:
  Containers:
  - name: envar-demo-container
    ...
    envFrom:
      - secretRef:
          name: manifest-example
...
````

- Volume mount :

````yml
spec:
  Containers:
  - name: envar-demo-container
    ...
    volumeMounts:
    - name: appConfigVol
      mountPath: /etc/config
  volumes:
  - name: appConfigVol
    secret:
      name: manifest-example
````

### Using secret to pull from a private registry

to pull images from your private registry you can declare a secret containing all the specifications of your private registry :

````sh
kubectl create secret docker-registry --docker-server=private-reg-container.com:5000 --docker-username=dock_user --docker-password=dock_password --docker-email=dock_user
@private-reg-container.com private-reg-cred
````

add the private registry so that it can fetch from it :

````yml
apiVersion: v1
kind: Pod
metadata:
  name: private-reg
spec:
  containers:
  - name: private-reg-container
    image: <your-private-image>
  imagePullSecrets:
  - name: private-reg-cred
````

### how kubernetes handles secrets

the way kubernetes handles secrets. Such as:

- A secret is only sent to a node if a pod on that node requires it.

- Kubelet stores the secret into a ``tmpfs`` so that the secret is not written to disk storage.

- Once the Pod that depends on the secret is deleted, kubelet will delete its local copy of the secret data as well.