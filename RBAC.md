todo
In a kubernetes cluster there is two types of ressources : 
- namespaced resources: like pods , deployments, .... there attached to a specific namespace. To manage those resources, we use roles and roles bindings
- cluster scoped : like nodes, pvs ... are not attached to any namespace . those resources are controlled with cluster roles and cluster role bindings

````sh
user --> attached with role binding -> role
````
same analogy to cluster binding .

example for cluster-role: 

````yml
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: storage-admin
rules:
- apiGroups: [""]
  resources: ["persistentvolumes"]
  verbs: ["get", "watch", "list", "create", "delete"]
- apiGroups: ["storage.k8s.io"]
  resources: ["storageclasses"]
  verbs: ["get", "watch", "list", "create", "delete"]
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: michelle-storage-admin
subjects:
- kind: User
  name: michelle
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: storage-admin
  apiGroup: rbac.authorization.k8s.io
````

>note: you can search for the API groups and resource names with ``kubectl api-resources``. 
