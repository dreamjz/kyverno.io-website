---
title: "Cleanup Empty ReplicaSets"
category: Other
version: 1.9.0
subject: ReplicaSet
policyType: "cleanUp"
description: >
    ReplicaSets are an intermediary controller to several Pod controllers such as Deployments. When a new version of a Deployment is created, it spawns a new ReplicaSet with the desired number of replicas and scale the current one to zero. This can have the effect of leaving many empty ReplicaSets in the cluster which can create clutter and false positives if policy reports are enabled. This cleanup policy removes all empty ReplicaSets across the cluster. Note that removing empty ReplicaSets may prevent rollbacks.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//cleanup/cleanup-empty-replicasets/cleanup-empty-replicasets.yaml" target="-blank">/cleanup/cleanup-empty-replicasets/cleanup-empty-replicasets.yaml</a>

```yaml
apiVersion: kyverno.io/v2beta1
kind: ClusterCleanupPolicy
metadata:
  name: cleanup-empty-replicasets
  annotations:
    policies.kyverno.io/title: Cleanup Empty ReplicaSets
    policies.kyverno.io/category: Other
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: ReplicaSet
    kyverno.io/kyverno-version: 1.11.1
    policies.kyverno.io/minversion: 1.9.0
    kyverno.io/kubernetes-version: "1.27"
    policies.kyverno.io/description: >-
      ReplicaSets are an intermediary controller to several Pod controllers such as Deployments. When a new version of a Deployment is created, it spawns a new ReplicaSet with the desired number of replicas and scale the current one to zero. This can have the effect of leaving many empty ReplicaSets in the cluster which can create clutter and false positives if policy reports are enabled. This cleanup policy removes all empty ReplicaSets across the cluster. Note that removing empty ReplicaSets may prevent rollbacks.
spec:
  match:
    any:
    - resources:
        kinds:
          - ReplicaSet
  conditions:
    all:
    - key: "{{ target.spec.replicas }}"
      operator: Equals
      value: 0
  schedule: "*/5 * * * *"

```
