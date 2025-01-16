## Day-03: Pods Not Schedulable

In Kubernetes, the scheduler is responsible for assigning pods to nodes in the cluster based on various criteria. Sometimes, you might encounter situations where pods are not being scheduled as expected. This can happen due to factors such as node constraints, pod requirements, or cluster configurations.

---

### **Node Selector:**
Node Selector is a simple way to constrain pods to specific nodes with matching labels. It works by specifying key-value pairs that the node's labels must match.

**Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
spec:
  containers:
  - name: my-app
    image: my-image
  nodeSelector:
    disktype: ssd
```

In this example, the pod will only be scheduled on nodes with the label `disktype: ssd`.

---

### **Node Affinity:**
Node Affinity is a more flexible and powerful way to specify rules about where pods should be scheduled based on node labels. It allows you to define rules that are **required** or **preferred** for scheduling.

**Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
spec:
  containers:
  - name: my-app
    image: my-image
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
            - key: disktype
              operator: In
              values:
                - ssd
```

This configuration ensures that the pod is scheduled only on nodes with the label `disktype: ssd`.

---

### **Taints and Tolerations:**
Taints are applied to nodes to repel certain pods unless they have matching tolerations. This is useful for ensuring that only specific pods are scheduled onto certain nodes.

To add a taint to a node:

```bash
kubectl taint nodes node1 disktype=ssd:NoSchedule
```

This taint prevents any pod from being scheduled on `node1` unless the pod has a matching toleration.

To apply tolerations in your pod configuration:

**Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
spec:
  containers:
  - name: my-app
    image: my-image
  tolerations:
  - key: disktype
    operator: Equal
    value: ssd
    effect: NoSchedule
```

In this example, the pod can be scheduled on nodes that have the taint `disktype=ssd:NoSchedule`.
