# DEMO outline

## Problem description

By default, Kubernetes clusters places a taint on each of the master nodes (`node-role.kubernetes.io/master`) to prevent user workloads from being scheduled on them.

However, also by default, there are no protections in place to prevent a user from adding a toleration for this taint, causing these workloads to run on the control plane.

Therefore, in this demo we plan to showcase how to leverage on OPA/Gatekeeper and Kyverno to prevent users creating container workloads on kubernetes nodes with the default master taint.  


## Available solutions

To follow the implementation based on OPA/Gatekeeper, just head to the [demo-gatekeeper](demo-gatekeeper.md) description.

If by contrast, interested in the implementation of the recent Kyverno, just follow the steps on the [demo-kyverno](demo-kyverno.md) description.

Enjoy! :)
