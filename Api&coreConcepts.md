# K8s API
<!-- MDTOC maxdepth:6 firsth1:1 numbering:1 flatten:0 bullets:1 updateOnSave:1 -->

- 1. [K8s API](#k8s-api)   
- 2. [API ?](#api)   
   - 2.1. [API Versioning](#api-versioning)   
   - 2.2. [API Groups](#api-groups)   
   - 2.3. [Object model](#object-model)   
      - 2.3.1. [Object expression](#object-expression)   
      - 2.3.2. [Workloads Object Model](#workloads-object-model)   
- 3. [Core Concepts](#core-concepts)   
   - 3.1. [Namespaces](#namespaces)   
   - 3.2. [Pods](#pods)   
   - 3.3. [Labels](#labels)   
   - 3.4. [Selectors](#selectors)   
   - 3.5. [Services](#services)   

<!-- /MDTOC -->
# API ?
The REST API is the **KeyStone** of kubernetes .All operations and communications between components, and external users are API calls that the API Server handles . An object oriented architecture is used .  
**Main format:**
````
/api/<version>
/apis/<group>/<version>/<resource>
````
## API Versioning
To eliminate fields or restructure resource representations, Kubernetes supports multiple API versions, each at a different API path.
The version is set at the API level rather than at the resource or field level to:  
- Ensure that the API presents a clear and consistent view of system resources and behavior.
- Enable control access to end-of-life and/or experimental APIs.
three main levels of stability in the API:

- `Alpha`:
    - **Disabled by Default**
    - Used by `alpha` word, example: `apiVersion: v1alpha1 `
    - Features can be **buggy**
    - The support of a **feature can be dropped** at any time and without any notice
    - The API can change in a way that the feature may not work anymore
    - For **testing purposes only**
- `Beta`:
    - Used by `beta` word, example: `apiVersion: v1beta3 `
    - **Enabled by Default**
    - The feature is **tested** and considered as **stable**
    - The support of the feature will not be dropped, though the details may change
    - **API schema may change**
    - **non-business-critical** uses because of potential for incompatible changes in subsequent releases
- `Stable`:
    - **Enabled by Default**
    - The version name is ``vX`` where X is an integer
    - **Released and stable** Feature with no change on the API schema
    - May be used on a production grade.
## API Groups
An API Group is a REST compatible path that acts as the type descriptor for a kubernetes object . API Groups make it easier to extend k8s API.
There is several types of groups used :  
- The core (also called legacy) group, which is at REST path ``/api/v1`` and is not specified as part of the ``apiVersion`` field, for example, ``apiVersion: v1``
- The named groups are at REST path ``/apis/$GROUP_NAME/$VERSION``, and use apiVersion: ``$GROUP_NAME/$VERSION`` (for example, ``apiVersion: batch/v1``  

Most used ones :
````sh
apiVersion: v1
  # for pods, volumes , pv, pvc,container
/apis/apps/v1/deployments
  # deployements
/apis/batch/v1beta1/cronjobs
 #cronjobs
````
## Object model
Objects are a persistant entity that represent the desired state of the object within the cluster . **EVERY** object **MUST** have ``apiVersion``, ``kind``, and poses the nested fields: ``metadata.name``, (can be inhereted )``metadata.namespace``, (can be generated Automatically)``metadata.UID``

- ``apiVersion``: Kubernetes API version of the Object
- ``kind``: Type of Kubernetes Object
- ``metadata.name``: Unique name of the Object
- ``metadata.namespace``: Scoped environment name that the object belongs to (will default to current).
- ``metadata.uid``: The (generated) uid for an object.
### Object expression
Files or other representations of k8s objects are generally represented in YAML.
2 main rules in writing YAML files :
- Uses white space alignment to denote ownership
- Three basic Data types :
    - Mappings: hash or dictionary,
    - Sequences: Array or list
    - Scalar: String, number, bool ...

![YAML types](assets/Api&coreConcepts-f9a1e.png)

### Workloads Object Model
Workload related objects within k8s have an additional two nested fields ``spec`` and ``status``  :
- ``status``: is managed by k8s and describes the actual state of the object and its history
- ``spec``: Describes the **desired state** or **configuration** of the object to be created  

![Object& Status Snippet](assets/Api&coreConcepts-0c703.png)


# Core Concepts
## Namespaces
## Pods

## Labels
## Selectors
## Services
