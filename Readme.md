# Helm

## Create Chart

```
helm create ChartName
```

## Install Chart
```
helm install <release-name> <chart-path> --dry-run --debug
helm install nginx . --values prod.yaml --dry-run --debug  
```


Best Practice for k8s manifest/charts

Manifest repo
  app-a-manifest
  app-b-manifest
  app-n-manifest

Ingress repo
  ingress.yaml

Because app each app have different manifest, but ingress has single host name
ingres.yaml
```
apiVersion: networking.k8s.io/v1

kind: Ingress

metadata:

  name: dev-grpc-ingress

  namespace: dev

  annotations:

    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"

  #   nginx.ingress.kubernetes.io/rewrite-target: /

spec:

  ingressClassName: nginx

  tls:

    - hosts:

      - dev.myapps.com

      secretName: idigicloud-local

  rules:

  - host: dev.myapps.com

    http:

      paths:

      - path: /api/app-a

        pathType: ImplementationSpecific

        backend:

          service:

            name: app-a

            port:

              name: http

      - path: /api/app-b

        pathType: ImplementationSpecific

        backend:

          service:

            name: app-b

            port:

              name: http

      - path: /api/app-n

        pathType: ImplementationSpecific

        backend:

          service:

            name: app-n

            port:

              name: http

```
so if i want to add new ingree i can just append 
```
      - path: /api/app-x

        pathType: ImplementationSpecific

        backend:

          service:

            name: app-x

            port:

              name: http
``` 
to ingres.yaml

what do you think? and what is recommended and best practise 

Im thinking of single repo and have multiple manifest init

manifestRepo
  - MainChart
  - appA
  - appB
  - appC

so that i can import maincart as dependency in a b c

each appX will consist of

deployment > imported from MainChart
Service > imported from MainChart
ConfigMap > might be custom

whadya think?