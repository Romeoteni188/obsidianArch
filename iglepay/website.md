primera version

```bas
docker build -t romeo188/iglepay-website:v1.0.0 .
docker push romeo188/iglepay-website:v1.0.0

```

para ver los certivados 

```bash
kubectl describe certificate iglepay-tls
```

```bash
kubectl describe challenge | grep mi.iglepay.com -A 5
```

- eliminar certificado
```bash
kubectl delete certificate iglepay-tls
```

- renovar
```bash
kubectl apply -f ingress.yaml
```

## para cargar variables

```bash
docker build -t romeo188/iglepay-website:v1.1.1 \
  --build-arg NEXT_PUBLIC_RECAPTCHA_SITE_KEY=6LdhwvIrAAAAAJR2Z-DJ1lFThaY_Op-1WhsujoGN \
  --build-arg NEXT_PUBLIC_GA_TRACKING_ID=G-SBV4N2SG91 .
docker push romeo188/iglepay-website:v1.1.1

```

aplicar cambios
```bash
kubectl apply -f website.yaml
```



