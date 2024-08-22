created and tested in Homelab 

---

# **Environment Setup**

### **Start and Check Vagrant**
1. **Start the environment:**
   ```sh
   vagrant up
   ```
2. **Check the status of the environment:**
   ```sh
   vagrant status
   ```

---

# **Accessing Kubernetes Nodes from a Windows Machine**

### **Install and Configure `kubectl` on Windows**
1. **Install `kubectl`:**
   - Download the `kubectl` binary.
   - Copy it to `C:\tools`.

2. **Set Environment Path:**
   - Add `C:\tools` to your system's environment `PATH` so `kubectl` can be run from any location.

3. **Configure Kubernetes:**
   - Create a `.kube` directory under your user home directory:  
     `C:\Users\<YourUsername>\.kube\`
   - Copy the `config` file from the `configs` directory to `C:\Users\<YourUsername>\.kube\`.

4. **Verify Cluster Access:**
   - Run the following command to check if you can access your Kubernetes nodes:
     ```sh
     kubectl get nodes
     ```
   - If this command doesn't work, ensure the `config` file is correctly placed in `C:\Users\<YourUsername>\.kube\`.

---

# **Accessing the Kubernetes Dashboard**

### **Start the Dashboard**
1. **Port-forward the Kubernetes Dashboard:**
   ```sh
   kubectl port-forward -n kubernetes-dashboard service/kubernetes-dashboard 8443:443
   ```

2. **Access the Dashboard:**
   - Open your browser and navigate to:  
     `https://localhost:8443/#/login`

3. **Troubleshooting:**
   - If you encounter any issues, you can delete all dashboard pods to reset them:
     ```sh
     kubectl delete pod -n kubernetes-dashboard --all
     ```

---

# **Installing and Accessing Prometheus Monitoring**

### **Install Helm and Prometheus Operator on Windows**

1. **Install Helm:**
   - Install Helm on Windows.
   - Copy the Helm binary to `C:\tools` so it can be accessed globally via the environment path.

2. **Install Prometheus Operator:**
   - Create a namespace for monitoring:
     ```sh
     kubectl create namespace monitoring
     ```
   - Add the Prometheus Community Helm repository:
     ```sh
     helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
     helm repo update
     ```
   - Install the Prometheus Operator Helm chart:
     ```sh
     helm install prometheus-operator prometheus-community/kube-prometheus-stack --namespace monitoring
     ```

3. **Verify the Installation:**
   - Check the running pods:
     ```sh
     kubectl get pods -n monitoring
     ```
   - Check the services:
     ```sh
     kubectl get svc -n monitoring
     ```

### **Access Prometheus and Alertmanager Dashboards**
1. **Access Prometheus:**
   - Port-forward Prometheus:
     ```sh
     kubectl port-forward -n monitoring svc/prometheus-operated 9090:9090
     ```
   - Open your browser and navigate to:  
     `http://localhost:9090`

2. **Access Alertmanager:**
   - Port-forward Alertmanager:
     ```sh
     kubectl port-forward -n monitoring svc/alertmanager-operated 9093:9093
     ```
   - Open your browser and navigate to:  
     `http://localhost:9093`

3. **Access Grafana:**
   - Port-forward Grafana:
     ```sh
     kubectl port-forward -n monitoring svc/prometheus-operator-grafana 3000:80
     ```
   - Open your browser and navigate to:  
     `http://localhost:3000`
   - Login with `admin` as the username and `prom-operator` as the password, or decode the password using:
     ```sh
     kubectl get secret -n monitoring prometheus-operator-grafana -o jsonpath="{.data.admin-password}" | base64 --decode
     ```

---

# **Installing Ingress NGINX via Helm**

### **Install Ingress NGINX**
1. **Install Ingress NGINX:**
   ```sh
   helm upgrade --install ingress-nginx ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --namespace ingress-nginx --create-namespace
   ```

### **Deploy a Hello World Application**

1. **Create Namespace and Deployment:**
   ```sh
   kubectl create namespace raj
   kubectl create deployment hello-world --image=gcr.io/google-samples/hello-app:1.0 -n raj
   ```

2. **Expose the Deployment:**
   ```sh
   kubectl expose deployment hello-world --type=ClusterIP --port=8080 -n raj
   ```

### **Access the Application**
- **Option 1: Ingress NGINX:**
  - If you've updated `/etc/hosts` to point `mydemourl.com` to localhost, you can access the application via the configured URL.
  
- **Option 2: Port Forwarding:**
  - Use port forwarding for local access:
    ```sh
    kubectl port-forward -n raj svc/hello-world 8080:8080
    ```

---

You can copy and paste this directly into your GitHub README file. The formatting should be preserved and provide clear instructions for setting up and using your environment.
