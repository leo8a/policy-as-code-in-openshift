# Demo outline for Kyverno


### OpenShift cluster info

The OpenShift cluster used during the integration was fresh deployed on AWS using the [Installer-Provisioned Infrastructure (IPI)](https://docs.openshift.com/container-platform/4.7/installing/installing_aws/installing-aws-customizations.html#installing-aws-customizations) method.
```shell
# check OpenShift cluster
oc get no,clusterversion,co

-- (output) --
NAME                                STATUS   ROLES    AGE   VERSION
node/ip-10-0-130-246.ec2.internal   Ready    worker   20m   v1.20.0+87cc9a4
node/ip-10-0-137-82.ec2.internal    Ready    master   32m   v1.20.0+87cc9a4
node/ip-10-0-144-114.ec2.internal   Ready    master   31m   v1.20.0+87cc9a4
node/ip-10-0-171-48.ec2.internal    Ready    master   30m   v1.20.0+87cc9a4

NAME                                         VERSION   AVAILABLE   PROGRESSING   SINCE   STATUS
clusterversion.config.openshift.io/version   4.7.19    True        False         2m28s   Cluster version is 4.7.19

NAME                                                                           VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE
clusteroperator.config.openshift.io/authentication                             4.7.19    True        False         False      2m31s
clusteroperator.config.openshift.io/baremetal                                  4.7.19    True        False         False      30m
clusteroperator.config.openshift.io/cloud-credential                           4.7.19    True        False         False      35m
clusteroperator.config.openshift.io/cluster-autoscaler                         4.7.19    True        False         False      29m
clusteroperator.config.openshift.io/config-operator                            4.7.19    True        False         False      30m
clusteroperator.config.openshift.io/console                                    4.7.19    True        False         False      12m
clusteroperator.config.openshift.io/csi-snapshot-controller                    4.7.19    True        False         False      29m
clusteroperator.config.openshift.io/dns                                        4.7.19    True        False         False      28m
clusteroperator.config.openshift.io/etcd                                       4.7.19    True        False         False      29m
clusteroperator.config.openshift.io/image-registry                             4.7.19    True        False         False      19m
clusteroperator.config.openshift.io/ingress                                    4.7.19    True        False         True       16m
clusteroperator.config.openshift.io/insights                                   4.7.19    True        False         False      24m
clusteroperator.config.openshift.io/kube-apiserver                             4.7.19    True        False         False      26m
clusteroperator.config.openshift.io/kube-controller-manager                    4.7.19    True        False         False      28m
clusteroperator.config.openshift.io/kube-scheduler                             4.7.19    True        False         False      27m
clusteroperator.config.openshift.io/kube-storage-version-migrator              4.7.19    True        False         False      17m
clusteroperator.config.openshift.io/machine-api                                4.7.19    True        False         False      22m
clusteroperator.config.openshift.io/machine-approver                           4.7.19    True        False         False      29m
clusteroperator.config.openshift.io/machine-config                             4.7.19    True        False         False      27m
clusteroperator.config.openshift.io/marketplace                                4.7.19    True        False         False      28m
clusteroperator.config.openshift.io/monitoring                                 4.7.19    True        False         False      15m
clusteroperator.config.openshift.io/network                                    4.7.19    True        False         False      29m
clusteroperator.config.openshift.io/node-tuning                                4.7.19    True        False         False      29m
clusteroperator.config.openshift.io/openshift-apiserver                        4.7.19    True        False         False      22m
clusteroperator.config.openshift.io/openshift-controller-manager               4.7.19    True        False         False      30m
clusteroperator.config.openshift.io/openshift-samples                          4.7.19    True        False         False      20m
clusteroperator.config.openshift.io/operator-lifecycle-manager                 4.7.19    True        False         False      29m
clusteroperator.config.openshift.io/operator-lifecycle-manager-catalog         4.7.19    True        False         False      29m
clusteroperator.config.openshift.io/operator-lifecycle-manager-packageserver   4.7.19    True        False         False      22m
clusteroperator.config.openshift.io/service-ca                                 4.7.19    True        False         False      30m
clusteroperator.config.openshift.io/storage                                    4.7.19    True        False         False      30m
```
```shell
# check present taints and labels (on a fresh install)
oc describe no | grep Taints:

-- (output) --
Taints:             <none>
Taints:             node-role.kubernetes.io/master:NoSchedule
Taints:             node-role.kubernetes.io/master:NoSchedule
Taints:             node-role.kubernetes.io/master:NoSchedule


oc describe no | grep Labels:

-- (ouput) --
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/instance-type=m4.large
                    beta.kubernetes.io/os=linux
                    failure-domain.beta.kubernetes.io/region=us-east-1
                    failure-domain.beta.kubernetes.io/zone=us-east-1a
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=ip-10-0-130-246
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/worker=
                    node.kubernetes.io/instance-type=m4.large
                    node.openshift.io/os_id=rhcos
--
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/instance-type=m4.xlarge
                    beta.kubernetes.io/os=linux
                    failure-domain.beta.kubernetes.io/region=us-east-1
                    failure-domain.beta.kubernetes.io/zone=us-east-1a
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=ip-10-0-137-82
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
                    node.kubernetes.io/instance-type=m4.xlarge
                    node.openshift.io/os_id=rhcos
--
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/instance-type=m4.xlarge
                    beta.kubernetes.io/os=linux
                    failure-domain.beta.kubernetes.io/region=us-east-1
                    failure-domain.beta.kubernetes.io/zone=us-east-1b
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=ip-10-0-144-114
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
                    node.kubernetes.io/instance-type=m4.xlarge
                    node.openshift.io/os_id=rhcos
--
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/instance-type=m4.xlarge
                    beta.kubernetes.io/os=linux
                    failure-domain.beta.kubernetes.io/region=us-east-1
                    failure-domain.beta.kubernetes.io/zone=us-east-1c
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=ip-10-0-171-48
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
                    node.kubernetes.io/instance-type=m4.xlarge
                    node.openshift.io/os_id=rhcos
```


### 1) Install [Kyverno](https://kyverno.io/docs/installation/#install-kyverno-using-yamls) via YAML resources
```shell
# install Kyverno
oc apply -f https://raw.githubusercontent.com/kyverno/kyverno/main/definitions/release/install.yaml
```
```shell
# check installed CRDs
oc get crd | grep "kyverno\|wgpolicyk8s"

-- (expected output) --
clusterpolicies.kyverno.io                                        2021-07-16T13:11:56Z
clusterpolicyreports.wgpolicyk8s.io                               2021-07-16T13:11:56Z
clusterreportchangerequests.kyverno.io                            2021-07-16T13:11:57Z
generaterequests.kyverno.io                                       2021-07-16T13:11:57Z
policies.kyverno.io                                               2021-07-16T13:11:57Z
policyreports.wgpolicyk8s.io                                      2021-07-16T13:11:57Z
reportchangerequests.kyverno.io                                   2021-07-16T13:11:58Z
```
```shell
oc get all -n kyverno

-- (expected output) --
NAME                          READY   STATUS    RESTARTS   AGE
pod/kyverno-59bbfc54d-7cssl   1/1     Running   0          68s

NAME                          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/kyverno-svc           ClusterIP   172.30.235.13    <none>        443/TCP    70s
service/kyverno-svc-metrics   ClusterIP   172.30.143.118   <none>        8000/TCP   70s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kyverno   1/1     1            1           69s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/kyverno-59bbfc54d   1         1         1       69s
```


### 2) Create Kyverno policies

#### 2.1) Controlling Scheduling (recommended)

a) create `ClusterPolicy` to ensure a `nodeSelector` corresponding to a non-control-plane node (i.e., nodes with the `node-role.kubernetes.io/worker=` label)
```shell
oc apply -f- << EOF
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: add-node-selector
  annotations:
    policies.kyverno.io/title: Add nodeSelector
    policies.kyverno.io/category: Sample
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
            Adds the nodeSelector field to a Pod spec.
spec:
  background: false
  rules:
  - name: add-node-selector
    match:
      resources:
        kinds:
        - Pod
    # Adds the `nodeSelector` field to any Pod with two labels.
    mutate:
      patchStrategicMerge:
        spec:
          nodeSelector:
            node-role.kubernetes.io/worker: ""
EOF
```

> NOTE: The above `ClusterPolicy` works even in the case of a single node, or “control plane only” cluster.  If the master nodes *are also* the worker nodes, then this arrangement permits user workloads to run as intended.


#### 2.2) Controlling Tolerations

a) create `ClusterPolicy` to avoid both (i.e., global and specific) tolerations on master nodes taints
```shell
oc apply -f- << EOF
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-control-plane-scheduling
  annotations:
    policies.kyverno.io/title: Restrict control plane scheduling
    policies.kyverno.io/category: Sample
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
      This policy prevents users from setting a toleration
      in a Pod spec which allows running on control plane nodes
      with the taint key "node-role.kubernetes.io/master".   
spec:
  validationFailureAction: enforce
  background: false
  rules:
  - name: restrict-control-plane-scheduling
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: Pods may not use tolerations which schedule on control plane nodes.
      pattern:
        spec:
          =(tolerations):
            - key: "!node-role.kubernetes.io/master"
EOF
```

b) create a pod with a global toleration
```shell
oc create -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  generateName: tolerated-pod-   # trailing hyphen is intentional
spec:
  tolerations:
  - operator: "Exists"
  containers:
  - image: docker.io/redhat/ubi8:latest
    name: demo
    command: ["/bin/sh", "-c", "while :; do date; sleep 10; done"]
EOF

-- (expected output) --
Error from server: error when creating "STDIN": admission webhook "validate.kyverno.svc" denied the request: 

resource Pod/default/tolerated-pod-2vm87 was blocked due to the following policies

restrict-control-plane-scheduling:
  restrict-control-plane-scheduling: 'validation error: Pods may not use tolerations
    which schedule on control plane nodes. Rule restrict-control-plane-scheduling
    failed at path /spec/tolerations/0/key/'
```

c) create a pod with specific toleration
```shell
oc create -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  generateName: tolerated-pod-   # trailing hyphen is intentional
spec:
  tolerations:
  - effect: NoSchedule
    key: node-role.kubernetes.io/master
    operator: Exists
  containers:
  - image: docker.io/redhat/ubi8:latest
    name: demo
    command: ["/bin/sh", "-c", "while :; do date; sleep 10; done"]
EOF

-- (expected output) --
Error from server: error when creating "STDIN": admission webhook "validate.kyverno.svc" denied the request: 

resource Pod/default/tolerated-pod-p4smv was blocked due to the following policies

restrict-control-plane-scheduling:
  restrict-control-plane-scheduling: 'validation error: Pods may not use tolerations
    which schedule on control plane nodes. Rule restrict-control-plane-scheduling
    failed at path /spec/tolerations/0/key/'
```

[comment]: <> (#### 2.3&#41; Setting maximums for pod grace periods)

[comment]: <> (TODO: Add a policy for https://kubernetes.io/docs/concepts/scheduling-eviction/node-pressure-eviction/)


#### 2.3) Controlling Grace Period for Pods

a) create `ClusterPolicy` to avoid pods with a `terminationGracePeriodSeconds` larger than 50 seconds
```shell
oc apply -f- << EOF
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-pod-grace-period
  annotations:
    policies.kyverno.io/title: Restrict termination grace period
    policies.kyverno.io/category: Sample
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
      This policy prevents users from setting a termination
      grace period in a Pod larger than 50 seconds.   
spec:
  validationFailureAction: enforce
  background: false
  rules:
  - name: restrict-pod-grace-period
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: Pods may not have a termination grace period larger than 50 seconds.
      pattern:
        spec:
          =(terminationGracePeriodSeconds): >50
EOF
```

b) create a pod with a larger `terminationGracePeriodSeconds`
```shell
oc apply -f- << EOF
apiVersion: v1
kind: Pod
metadata:
  name: grace-period-pod
spec:
  containers:
  - image: docker.io/redhat/ubi8:latest
    name: demo
    command: ["/bin/sh", "-c", "while :; do date; sleep 10; done"]
  terminationGracePeriodSeconds: 60
EOF

-- (expected output) --
Error from server: error when creating "STDIN": admission webhook "validate.kyverno.svc" denied the request: 

resource Pod/default/grace-period-pod was blocked due to the following policies

restrict-pod-grace-period:
  restrict-pod-grace-period: 'validation error: Pods may not have a termination grace
    period larger than 50 seconds. Rule restrict-pod-grace-period failed at path /spec/terminationGracePeriodSeconds/'

```


## Bonus

a) create `ClusterPolicy` to restrict users from scheduling pods by using the `nodeSelector` and `nodeName` fields
```shell
oc apply -f- << EOF
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-node-selection
  annotations:
    policies.kyverno.io/title: Restrict node selection
    policies.kyverno.io/category: Sample
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
      This policy prevents users from targeting specific nodes
      for scheduling of Pods by prohibiting the use of the `nodeSelector`
      and `nodeName` fields in a Pod spec.           
spec:
  validationFailureAction: enforce
  background: false
  rules:
  - name: restrict-node-selector
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: Setting the nodeSelector field is prohibited.
      pattern:
        spec:
          X(nodeSelector): "null"
  - name: restrict-node-name
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: Setting the nodeName field is prohibited.
      pattern:
        spec:
          X(nodeName): "null"
EOF
```

b) create a pod with a `nodeSelector`
```shell
oc apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: node-selector-pod
spec:
  nodeSelector:
    node-role.kubernetes.io/any-other-node: ""
  containers:
  - image: docker.io/redhat/ubi8:latest
    name: demo
    command: ["/bin/sh", "-c", "while :; do date; sleep 10; done"]
EOF

-- (expected output) --
Error from server: error when creating "STDIN": admission webhook "validate.kyverno.svc" denied the request: 

resource Pod/default/node-selector-pod was blocked due to the following policies

restrict-node-selection:
  restrict-node-selector: 'validation error: Setting the nodeSelector field is prohibited.
    Rule restrict-node-selector failed at path /spec/nodeSelector/'
```

c) create a pod with a `nodeName`
```shell
oc apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: node-name-pod
spec:
  nodeName: any-other-node
  containers:
  - image: docker.io/redhat/ubi8:latest
    name: demo
    command: ["/bin/sh", "-c", "while :; do date; sleep 10; done"]
EOF

-- (expected output) --
Error from server: error when creating "STDIN": admission webhook "validate.kyverno.svc" denied the request: 

resource Pod/default/node-name-pod was blocked due to the following policies

restrict-node-selection:
  restrict-node-name: 'validation error: Setting the nodeName field is prohibited.
    Rule restrict-node-name failed at path /spec/nodeName/'
```

> NOTE: Using Kyverno policies, we can have a simple (yet powerful) way to define policies using YAML resources. This simplicity also helps to keep secure and easy to read the policies within the cluster.
