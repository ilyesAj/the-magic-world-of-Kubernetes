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
    - name: DEMO_GREETING
      value: "Hello from the environment"
    - name: DEMO_FAREWELL
      value: "Such a sweet sorrow"
````

## ConfigMap 