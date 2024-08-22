how to start enviorment 
=======================================
vagrant up 
vagrant status 



======================================================
kubectl how to access from windows machine your nodes kuebctl 
=====================================================
1. install kubectl   under c drive c:/tools
2. then copy kubctl to there 
3. set enviroment path c:/tools so kubectl can run from any where in pc 
5. create .kube folder under user home   c:/users/rajesh/kube 
4. from this direcotry under configs folder there is config file this need to be copied to c:/user/rajes/.kube/
6. run the command now you can able to see your cluster nodes 

===========================================================
kubectl get nodes will not work you need to copy config file 
=============================================================
copy config file from  folder configs/config   to  c:/user/rajes/.kube/

then run kubectl get nodes it should work now 


========================================================================
how to access dashbaord 
========================================================================
1. kubectl port-forward -n kubernetes-dashboard service/kubernetes-dashboard 8443:443
2. on browser hit this https://localhost:8443/#/login

if any issues delete the dasboard pods 
kubectl delete pod -n kubernetes-dashboard --all

then run 


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
how to install promehtoues and access monitoirng 
+++++++++++++++++++++++++++++++++++++++++++++++++++++++

1. install helm on windows and copy help c:/tool folder so env already set for this 

2. Step 1: Install the Prometheus Operator
Create a namespace for monitoring:

kubectl create namespace monitoring
Install the Prometheus Operator:

Use the official Helm chart to install the Prometheus Operator. First, add the Prometheus Community Helm repository:

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update


Install the Prometheus Operator Helm chart:

helm install prometheus-operator prometheus-community/kube-prometheus-stack --namespace monitoring

Verify the Installation
Check the pods:

kubectl get pods -n monitoring
You should see several pods running, including Prometheus, Alertmanager, Grafana, and others.

Check the services:
kubectl get svc -n monitoring

Note the services for Prometheus and Alertmanager. By default, these services are exposed within the cluster.

Step 3: Access Prometheus and Alertmanager Dashboards
To access the Prometheus and Alertmanager dashboards, you can use port forwarding.

Port forward Prometheus:*********************

kubectl port-forward -n monitoring svc/prometheus-operated 9090:9090
Access Prometheus by navigating to http://localhost:9090 in your web browser.

Port forward Alertmanager: ************************

kubectl port-forward -n monitoring svc/alertmanager-operated 9093:9093
Access Alertmanager by navigating to http://localhost:9093 in your web browser.


get grafana dashbaord :(********************)

kubectl port-forward -n monitoring svc/prometheus-operator-grafana 3000:80

open console http://localhost:3000
admin   and password prom-operator

or you can get it by running :  kubectl get secret -n monitoring prometheus-operator-grafana -o jsonpath="{.data.admin-password}"
its base64 value please decrty it 




+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
============== ingress nginx instlation via helm 
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

helm upgrade --install ingress-nginx ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --namespace ingress-nginx --create-namespace


install hellow world app 

kubectl create namespace raj
kubectl create deployment hello-world --image=gcr.io/google-samples/hello-app:1.0 -n raj
kubectl expose deployment hello-world --type=ClusterIP --port=8080 -n raj

if you updated /etc/host  to point   mydemourl.com  so local host access accsssibel form that route via ingress nginx if not use 

alternative way to access for testing perpose using kubectl port fowrward
kubectl port-forward -n raj svc/hello-world 8080:8080




