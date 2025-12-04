# Argo CD Helm Installation Guide

This guide walks you through installing **Argo CD** on a Kubernetes cluster using **Helm**. It assumes you already have:

* A `ClusterIssuer` applied for TLS certificates
* An `argocd-values.yaml` file containing your Argo CD configuration

---

## Prerequisites

* Kubernetes cluster (v1.25+ recommended)
* Helm v3+
* `kubectl` configured to access your cluster
* Cert-manager installed on the cluster

> ⚠️ **Important:** Ensure your ClusterIssuer is applied **before** installing Argo CD. Otherwise, TLS certificate generation may fail.

---

## Step 1: Apply the ClusterIssuer

```bash
kubectl apply -f cluster-issuer.yaml
```

---

## Step 2: Verify the ClusterIssuer

```bash
kubectl get clusterissuer
```

You should see your ClusterIssuer listed with a `READY` status.

---

## Step 3: Add the Argo CD Helm Repository

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

---

## Step 4: Install Argo CD Using Helm

```bash
helm install argocd argo/argo-cd -f argocd-values.yaml --namespace argocd --create-namespace
```

---

## Step 5: Verify the Installation

```bash
kubectl get pods -n argocd
kubectl get svc -n argocd
kubectl get ingress -n argocd
```

---

## Step 6: Retrieve the Initial Argo CD Admin Password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```

---

✅ **Argo CD is now installed and ready to use.** Access it via your configured Ingress or Service URL.
