para escalar por trafico y memoria
primero con deployment
frontend.yml

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: frontend
          image: romeo188/iglepay-frontend:v1.0.12
          imagePullPolicy: Always
          ports:
            - containerPort: 3000
          env:
            - name: TZ
              value: America/Guatemala
          envFrom:
            - configMapRef:
                name: frontend-config
            - secretRef:
                name: frontend-secret
          resources:
            requests:
              cpu: 100m
              memory: 128Mi
            limits:
              cpu: 300m
              memory: 256Mi
          readinessProbe:
            httpGet:
              path: /
              port: 3000
            initialDelaySeconds: 5
            periodSeconds: 10
            timeoutSeconds: 2
            failureThreshold: 3
          livenessProbe:
            httpGet:
              path: /
              port: 3000
            initialDelaySeconds: 15
            periodSeconds: 20
            timeoutSeconds: 2
            failureThreshold: 3
---
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  selector:
    app: frontend
  ports:
    - port: 80
      targetPort: 3000
  type: ClusterIP

```

Creacion del archivo frontend-hpa.yaml

frontend-hpa.yaml

```bash
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: frontend-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: frontend
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 60

```

aplicar cambios

```bash
kubectl apply -f frontend.yaml
kubectl rollout restart deployment frontend
kubectl apply -f frontend-hpa.yaml
```

ver estados de hpa

```bash
kubectl get hpa
kubectl describe hpa frontend-hpa
```

monitorear en vivo
```bash
watch -n 2 kubectl top pods -l app=frontend
```

## backend

ahora toca modificar el backend 
asi esta ahora yml simple con replica quemada

```bash

apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
        - name: backend
          image: romeo188/iglepay-backend:v1.0.4
          imagePullPolicy: Always
          ports:
            - containerPort: 4000
          env:
            - name: TZ
              value: America/Guatemala
          envFrom:
            - secretRef:
                name: backend-secrets
---
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  selector:
    app: backend
  ports:
    - port: 4000
  type: ClusterIP

```

ahora con HPA

- Agregar `resources.requests` y `limits`.
    
- Agregar `readinessProbe` y `livenessProbe`.
    
- Crear HPA con un mínimo de 2 pods y máximo de 10 pods, CPU como métrica.
este archivo se llama backend-hpa.yaml 
backend-hpa.yaml 
```bash
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: backend-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: backend
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50

```

cambios en archivo deploymen backend
  backend.yaml 
```bash

apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
        - name: backend
          image: romeo188/iglepay-backend:v1.0.5
          imagePullPolicy: Always
          ports:
            - containerPort: 4000
          env:
            - name: TZ
              value: America/Guatemala
          envFrom:
            - secretRef:
                name: backend-secrets
          resources:
            requests:
              cpu: 150m
              memory: 256Mi
            limits:
              cpu: 500m
              memory: 512Mi
          readinessProbe:
            httpGet:
              path: /api/v1/health
              port: 4000
            initialDelaySeconds: 5
            periodSeconds: 10
            timeoutSeconds: 2
            failureThreshold: 3
          livenessProbe:
            httpGet:
              path: /api/v1/health
              port: 4000
            initialDelaySeconds: 15
            periodSeconds: 20
            timeoutSeconds: 2
            failureThreshold: 3
---
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  selector:
    app: backend
  ports:
    - port: 4000
  type: ClusterIP
```

aplicar cambios

```bash
kubectl apply -f backend.yaml
kubectl apply -f backend-hpa.yaml
kubectl rollout restart deployment backend
```

### a) `kubectl get hpa`

```bash
kubectl get hpa
```

| NAME      | Nombre del HPA                               |
|-----------|----------------------------------------------|
| REFERENCE | Objeto que escala (Deployment, StatefulSet…) |
| TARGETS   | Métricas actuales vs objetivo                |
| MINPODS   | Min replicas                                 |
| MAXPODS   | Max replicas                                 |
| REPLICAS  | Número actual de pods                        |
| AGE       | Tiempo desde que se creó el HPA              |

### b) `kubectl describe hpa backend-hpa`

```bash
kubectl describe hpa backend-hpa
```

Muestra informacion detallada

- Métricas actuales y objetivos.
    
- Condiciones de escalado (`ScalingActive`, `ScalingLimited`, `AbleToScale`).
    
- Eventos recientes de HPA (errores, problemas de métricas).
    
- Número recomendado de replicas según la métrica actual.

