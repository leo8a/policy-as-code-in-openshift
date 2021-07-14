# Problem description

By default, Kubernetes clusters places a taint on each of the master nodes (node-role.kubernetes.io/master) to prevent user workloads from being scheduled on them.

However, also by default, there are no protections in place to prevent a user from adding a toleration for this taint, causing these workloads to run on the control plane.

Therefore, in this demo we plan to showcase how to leverage on OPA/Gatekeeper and Kyverno to prevent users creating container workloads on kubernetes nodes with the default master taint.  
