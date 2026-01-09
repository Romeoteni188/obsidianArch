
```bash
docker run --rm -t \
  -v $(pwd):/zap/wrk \
  zaproxy/zap-stable zap-baseline.py \
  -t https://data.10enconta.com \
  -r reporte.html \
  -m 5
```

