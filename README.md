# Kubernetes Troubleshooting - Zero to Hero

## Overview

This guide is designed to help you troubleshoot common issues encountered while working with Kubernetes. It covers essential troubleshooting techniques and the most frequent problems you'll come across in a Kubernetes environment.

---

## Day-01: **ImagePullBackOff**

### What is ImagePullBackOff?
**ImagePullBackOff** occurs when Kubernetes is unable to pull the required container image for a pod. This could be due to an incorrect image name or a lack of necessary credentials to access a private registry. Kubernetes will attempt to pull the image multiple times with increasing delays, but if it fails repeatedly, it enters this state.

### Solution:
- Verify the image name and tag.
- Ensure access to private registries by providing the correct credentials.

---

## Day-02: **CrashLoopBackOff**

### What is CrashLoopBackOff?
**CrashLoopBackOff** happens when a container within a pod repeatedly crashes after starting. Kubernetes tries to restart the container multiple times, but the application inside the container fails each time. The system enters a loop of continuous restarts, which leads to the **CrashLoopBackOff** state.

### Solution:
- Investigate the application’s logs and configuration.
- Ensure that the container is set up correctly and all necessary dependencies are met.

---

## Day-03: **Pods Not Schedulable**

There are several reasons why a pod might not be scheduled onto a node. This could be related to scheduling constraints such as:

### Node Selector:
A **nodeSelector** restricts the pod to run only on nodes that have specific labels. If no nodes meet the criteria, the pod will remain unscheduled.

### Node Affinity:
**Node Affinity** gives more flexibility in scheduling, as it allows you to specify rules about which nodes your pod should be scheduled to based on node labels.

### Taints and Tolerations:
A **taint** on a node can prevent pods from being scheduled to it unless the pod has a matching **toleration**. Taints can be used to repel pods from nodes unless they meet specific criteria, such as special requirements or conditions.

### Solution:
- Check for scheduling constraints like **nodeSelector**, **Node Affinity**, and **Taints**.
- Ensure that the pod and node configurations are aligned, including labels, tolerations, and resource requirements.
