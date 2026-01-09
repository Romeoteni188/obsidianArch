
para descargar el archivo de las ips fail2ban

```bash
scp -P 2222 -i ~/.ssh/id_ed25519 debian@148.113.203.34:/home/debian/banned_ips.txt .
```

para generar el archivo en vps

```bash
sudo fail2ban-client status sshd | grep 'Banned IP list' | cut -d: -f2 | tr ' ' '\n' > banned_ips.txt

```

