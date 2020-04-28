
# security contexts

security context are used to specify a user different than root when running a pod or a container.
If you want to add a security context applied to all containers of the pod , make sure to add it under ``spec:`` :

````yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
````

if you want to apply security context to one specific container , add security context under the ``container:``

>**note:**if security context is implemented within the pod and the container, the security context applied to the container directly will the one to be used by k8s .

you can use to ``AllowPrivilegeEscalation: false`` to prevent a process gaining more privileges than its parent process.

you can add a special capabilities to a container using the lines below within the container spec:

````yml
...
    securityContext:
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
...
````

# Network policy
A network policy is a specification of how groups of pods are allowed to communicate with each other and other network endpoints.

NetworkPolicy resources use labels to select pods and define rules which specify what traffic is allowed to the selected pods.
two types of traffic :

- ``ingress`` : who can communicate with me (we use ``from`` selector)

- ``egress`` : with who i will communicate (we use ``to`` selector)

> **Responses are not important** , you have to identify the direction of the traffic and secure it .

**By default,** pods are non-isolated; they accept traffic from any source.

simple example of managing traffic :

- For pods with `internal` label, allow egress traffic to `mysql` and  `payroll`

````yml
apiVersion: network.k8s.io/v1
kind: NetworkPolicy
metadata: 
  name: internal-policy
spec: 
  podSelector: 
    matchLabels:
      name: internal
  policyTypes:
    - Egress
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306
````
> Flannel does not support network policies, you will not have an error message for that ❗️ the network policies will simply not work .

# Securing secrets

TODO
talking about vault as a kms and how to implement it.

# references

https://kubernetes.io/docs/concepts/services-networking/network-policies/