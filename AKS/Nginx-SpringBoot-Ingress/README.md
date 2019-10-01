# Overview
Ingress controller provides reverse proxy, traffic routing, and TLS termination for Kubernetes services. This example shows how to speficially configure your Spring application and generally any application along with Nginx configuration and deployment to implement path based traffic routing

# Getting started
Deploy AKS cluster:
https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough
Install Tiller and Helm:
https://docs.microsoft.com/en-us/azure/aks/kubernetes-helm
Create Nginx ingress controller: 
https://docs.microsoft.com/en-us/azure/aks/ingress-tls

## First:
Deploy your application:
kubectl apply -f readinglistk8s.yaml

Note that I have set the application context path via an environmnet variable 
    - name: SERVER_SERVLET_CONTEXT_PATH
      value: /reading-list

Create an ingress route:
kubectl apply -f aks-nginx-ingress.yaml

Note that, I have configured the route such that any requests arriving with the path /reading-list/* will be routed to a kubernetes service named "web", which servers our front end application
  - http:
      paths:
      - backend:
          serviceName: web
          servicePort: 80
        path: /reading-list/*

## Then:
Retrive the Nginx ingress controller EXTERNAL-IP
kubectl get service

Access the application
http://EXTERNAL-IP/reading-list/home

