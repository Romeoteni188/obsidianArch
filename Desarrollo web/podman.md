cuando da el error 


❯ podman kube play   maria-db-pod.yml
Error: building local pause image: committing container for step {Env:[PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin] Command:entrypoint Args:[/catatonit -P] Flags:[] Attrs:map[json:true] Message:ENTRYPOINT /catatonit -P Heredocs:[] Original:ENTRYPOINT ["/catatonit", "-P"]}: obtaining default signature policy: no policy.json file found at any of the following: "/home/romeo188/.config/containers/policy.json", "/etc/containers/policy.json"



crear el archivo

mkdir -p ~/.config/containers
nano ~/.config/containers/policy.json



colocar en archivo

{
    "default": [
        {
            "type": "insecureAcceptAnything"
        }
    ]
}


====
podman pod ls

podman pod rm name

construiimar 
❯ podman kube play maria-db-pod.yml
levantar 
❯ podman pod start finanssoreal-pod


un servidcio con system.d
nano ~/.config/systemd/user/mariadb-pod.service

recargar y reinicar el servico 

systemctl --user daemon-reload
systemctl --user restart mariadb-pod.service


