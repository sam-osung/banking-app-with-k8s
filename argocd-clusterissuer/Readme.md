# Argo CD Helm Installation Guide

This guide walks you through installing **Argo CD** on a Kubernetes cluster using **Helm**. It assumes you already have:

- A `ClusterIssuer` applied for TLS certificates  
- A `argocd-values.yaml` file with your Argo CD configuration

---

## **Prerequisites**

- Kubernetes cluster (v1.25+ recommended)  
- Helm v3+  
- `kubectl` configured to access your cluster  
- Cert-manager installed on the cluster  

> ⚠️ **Important:** Ensure your ClusterIssuer is applied **before** installing Argo CD, otherwise TLS certificate generation will fail.

## **Step 1: Apply the ClusterIssuer**

```bash
kubectl apply -f cluster-issuer.yaml

---

## **Step 1: Verify that ClusterIssuer is running**

```bash
kubectl get clusterissuer


## You should see your ClusterIssuer listed with a `READY` status.


## **Step 2: Add the Argo CD Helm repository**

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update


## **Step 3: Install Argo CD using Helm**

```bash
helm install argocd argo/argo-cd -f argocd-values.yaml --namespace argocd --create-namespace


## **Step 4: Verify Installation**

```bash
kubectl get pods -n argocd
kubectl get svc -n argocd
kubectl get ingress -n argocd


# To get argocd password
# kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d