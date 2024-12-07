# Service DNS

# Step 1: Verify the Service and Namespace

Ensure the nginx-service exists in the my-namespace namespace:

kubectl get services -n my-namespace

If the service is not listed, create it or check the configuration. Here's an example of how to create a simple nginx service:


apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: my-namespace
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80

Apply the service definition:


kubectl apply -f nginx-service.yaml

# Step 2: Verify the DNS Configuration

Ensure the DNS pod (typically coredns) is running in the kube-system namespace:


kubectl get pods -n kube-system -l k8s-app=kube-dns

If the DNS pods are not running, troubleshoot them by describing the pods:

kubectl describe pod <dns-pod-name> -n kube-system

# Step 3: Test DNS Resolution Within the Cluster

Run a temporary pod to test DNS resolution:

kubectl run -i --tty dnsutils --image=infoblox/dnstools --restart=Never --rm

Inside the pod, test DNS resolution:

nslookup nginx-service.my-namespace.svc.cluster.local
