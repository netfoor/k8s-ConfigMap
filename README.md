# Kubernetes ConfigMap Project

This project demonstrates how to use Kubernetes ConfigMaps to manage configuration data for applications. The project involves creating two deployments (`blue-web` and `green-web`) running Nginx servers, each configured with its own ConfigMap to serve unique `index.html` files.

## Steps Followed

### 1. Create ConfigMaps

We created ConfigMaps to store the `index.html` files for the `blue-web` and `green-web` applications. These ConfigMaps were used to inject the HTML files into the Nginx containers.

```bash
kubectl create configmap blue-web-cm blue/index.html
kubectl create configmap green-web-cm green/index.html
```

### 2. Create Deployments

We created Kubernetes Deployment YAML files (`web-blue-with-cm.yaml` and `web-green-with-cm.yaml`) to define the Nginx containers. Each deployment specifies:

*   A volume that uses the respective ConfigMap.
*   A volume mount to inject the `index.html` file into the Nginx container at `/usr/share/nginx/html`.

Apply the deployments:

```bash
kubectl apply -f web-blue-with-cm.yaml
kubectl apply -f web-green-with-cm.yaml
```

### 3. Expose Deployments as Services

We exposed the deployments as NodePort services to make them accessible outside the cluster.

```bash
kubectl expose deployment blue-web --type=NodePort --name=blue-web-service
kubectl expose deployment green-web --type=NodePort --name=green-web-service
```

### 4. Access the Applications

Using Minikube, we listed all services and accessed the applications running on their respective ports.

List services:

```bash
kubectl get services
```

Access the applications:

*   `blue-web`: Visit the NodePort URL for the `blue-web-service`.
*   `green-web`: Visit the NodePort URL for the `green-web-service`.

### 5. Use Minikube Tunnel

To simplify access to services, we used Minikube's tunnel feature:

```bash
minikube tunnel
```

This allows services to be accessed using their cluster IPs.

## Benefits of Using ConfigMaps

*   **Separation of Configuration and Code:** ConfigMaps allow you to manage configuration data separately from application code.
*   **Dynamic Updates:** ConfigMaps can be updated without rebuilding or redeploying the application.
*   **Reusability:** ConfigMaps can be reused across multiple pods or deployments.
*   **Simplified Management:** Centralized configuration management simplifies updates and maintenance.

## Files in This Project

*   `web-blue-with-cm.yaml`: Deployment for the `blue-web` application.
*   `web-green-with-cm.yaml`: Deployment for the `green-web` application.
*   `blue/index.html`: HTML file for the `blue-web` application.
*   `green/index.html`: HTML file for the `green-web` application.

## Conclusion

This project demonstrates how to use ConfigMaps to inject configuration data into Kubernetes deployments. By leveraging ConfigMaps, we achieved a clean separation of configuration and application logic, making the deployments more flexible and easier to manage.
```