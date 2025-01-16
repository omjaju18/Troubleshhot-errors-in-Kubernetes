# Kubernetes Network Policies

This repository explains the various types of **Kubernetes Network Policies** and how they can be used to control network traffic between Pods, Services, and external resources in your Kubernetes cluster.

## Overview of Network Policies

In Kubernetes, **Network Policies** are used to manage how Pods can communicate with each other and with external resources. These policies allow you to control **ingress** (incoming traffic) and **egress** (outgoing traffic) for Pods based on certain rules, such as labels, IP ranges, and namespaces.

## Types of Network Policies

### 1. **Ingress Network Policy**

An **Ingress Network Policy** controls incoming traffic to Pods. It defines which Pods, Services, or external IPs are allowed to send traffic to the selected Pods.

#### Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-policy
spec:
  podSelector:
    matchLabels:
      app: myapp
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 80
```

**Explanation:**
- The policy applies to Pods with the label `app: myapp`.
- It allows incoming traffic only from Pods with the label `role: frontend` on port 80.

### 2. **Egress Network Policy**

An **Egress Network Policy** controls the outgoing traffic from Pods. It restricts the Pods to access only specific external resources, Services, or other Pods.

#### Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: egress-policy
spec:
  podSelector:
    matchLabels:
      app: myapp
  egress:
    - to:
        - podSelector:
            matchLabels:
              role: database
      ports:
        - protocol: TCP
          port: 3306
```

**Explanation:**
- The policy applies to Pods with the label `app: myapp`.
- It allows outgoing traffic only to Pods with the label `role: database` on port 3306 (MySQL).

### 3. **Ingress and Egress Network Policy**

A combined **Ingress and Egress Network Policy** manages both incoming and outgoing traffic in a single policy.

#### Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-egress-policy
spec:
  podSelector:
    matchLabels:
      app: myapp
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 80
  egress:
    - to:
        - podSelector:
            matchLabels:
              role: database
      ports:
        - protocol: TCP
          port: 3306
```

**Explanation:**
- Incoming traffic is allowed only from Pods with the label `role: frontend` on port 80.
- Outgoing traffic is allowed only to Pods with the label `role: database` on port 3306.

### 4. **Deny All Traffic Policy**

A **Deny All Traffic Policy** blocks all incoming and outgoing traffic to Pods by default unless other policies explicitly allow traffic.

#### Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-policy
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

**Explanation:**
- The policy applies to all Pods (`podSelector: {}`).
- Both ingress and egress traffic are denied by default until explicitly allowed.

### 5. **Allow All Traffic Policy**

An **Allow All Traffic Policy** allows all incoming and outgoing traffic to and from Pods, which is the default behavior in Kubernetes if no Network Policy is defined.

#### Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-all-policy
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

**Explanation:**
- This policy applies to all Pods (`podSelector: {}`).
- It allows both ingress and egress traffic from any source.

### 6. **Pod-to-Pod Network Policies**

A **Pod-to-Pod Network Policy** restricts traffic between Pods, controlling communication between different services or tiers of your application.

#### Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: restrict-db-access
spec:
  podSelector:
    matchLabels:
      app: database
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: web
```

**Explanation:**
- The policy applies to Pods with the label `app: database`.
- It allows incoming traffic only from Pods with the label `app: web`.

## Key Points to Remember

- **Pod Selector:** Specifies the Pods to which the policy applies.
- **Ingress and Egress:** Policies can be created for incoming traffic (Ingress) and outgoing traffic (Egress).
- **Policy Types:** You can have `Ingress`, `Egress`, or both in a single policy.
- **Allow and Deny:** Network policies can either allow or deny traffic based on specific rules.
  
Network Policies are essential for securing your Kubernetes cluster by ensuring that only authorized traffic is allowed between Pods. By using these policies, you can implement a least-privilege approach and tightly control communication in your applications.