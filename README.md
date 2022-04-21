# Example of K8S Ingress V1 Deprecation 

Starting with Kubernetes 1.22 the v1beta1 Ingress API will no longer be accepted.  Any Ingress YAML files with API Version v1Beta1 will need to be updated to V1.  The major change between beta and V1 is the way paths and backends are formatted.  

## What will happen when Kubernetes is upgraded to v1.22?

- Attempting to apply a ingress yaml manifest to Kubernetes will fail with an error.
- Existing ingress that has already been deployed will automatically be upgraded to V1 and continue to work as expected.  

## What do you need to do?
- Change the format of ingress.yaml mainfest files in your repos or in CI/CD to the V1 format.  

## Reference Docs
https://kubernetes.io/docs/concepts/services-networking/ingress/


## Example of Deprecated v1Beta Ingress
```YAML
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: demo-ingress
  namespace: demo
  
spec:
  rules:
  - host: demo.something.com
    http:
      paths:
      - backend:
          serviceName: demo-service
          servicePort: 80

  tls:
  - hosts:
    - demo.something.com
    secretName: certbot-demo
```

## Example of Current V1 Ingress
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-ingress
  annotations:
    kubernetes.io/ingress.class: "nginx"

spec:
  rules:
  - host: demo.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: demo-service
            port: 
              number: 80


  tls:
  - hosts:
    - demo.local
    secretName: certbot-demo
```
