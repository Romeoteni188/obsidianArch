
```bash
 openssl passwd 10enconta.com
$1$4xia/r9N$EwC.CdQ0.BBazjRXAYZWd.
```


use este

```bash
echo -n "MI_API_KEY" | openssl dgst -sha256 -binary | openssl base64 -A

WV6d6rnkiqXRsGvNVLcwtQPM7U9Gy6UT7AEL7A7aY6Y=%                                                                                                                                 
```

para ver si httonly funciona 

```bash
document.cookie
```

no debiera mostrar nada

variable CSRF
```bash
 echo -n "10encontaCSRF" |  openssl base64
```

```bash
MTBlbmNvbnRhQ1NSRg==
```



