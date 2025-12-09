# Example Kubernetes Manifests for GitOps Operator

This directory contains example Kubernetes manifests that demonstrate how the GitOps operator works.

## Files

- **configmap.yaml** - ConfigMap containing the HTML content for the Nginx web server
- **deployment.yaml** - Nginx deployment that mounts the ConfigMap as a volume
- **service.yaml** - Service to expose the Nginx deployment

## How It Works

1. The **ConfigMap** (`nginx-html`) contains an `index.html` file with HTML content
2. The **Deployment** creates 2 Nginx pods that mount the ConfigMap as a volume at `/usr/share/nginx/html`
3. The **Service** exposes the Nginx pods internally within the cluster

## Usage

### Option 1: Use with GitOps Operator

1. Push these files to a GitHub repository
2. Configure your GitOps operator to point to that repository
3. The operator will automatically discover and apply these manifests

### Option 2: Apply Manually

```bash
kubectl apply -f configmap.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

## Testing

After deployment, you can test the service:

```bash
# Port forward to access the service
kubectl port-forward service/nginx-service 8080:80

# Then visit http://localhost:8080 in your browser
```

Or create a temporary pod to test:

```bash
kubectl run curl-test --image=curlimages/curl --rm -it --restart=Never -- curl http://nginx-service
```

## Customizing

To customize the HTML content:

1. Edit the `index.html` content in `configmap.yaml`
2. Commit and push to your Git repository
3. The GitOps operator will automatically detect the change and update the ConfigMap
4. The Nginx pods will automatically reload with the new content

## Notes

- The manifests use the `default` namespace. You can change this if needed.
- The deployment creates 2 replicas for high availability.
- The service uses ClusterIP, so it's only accessible within the cluster. Use port-forwarding or an Ingress to expose it externally.

