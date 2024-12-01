# DNS over TLS Setup with Unbound

## Configuration Steps

### 1. Install Unbound

```bash
sudo apt install unbound
```

### 2. Configure Unbound

Edit: `/etc/unbound/unbound.conf`

```yaml
server:
    # Network
    interface: tailscale0
    port: 5335
    do-ip4: yes
    do-udp: yes
    do-tcp: yes
    do-ip6: no

    # Security
    tls-cert-bundle: /etc/ssl/certs/ca-certificates.crt
    harden-glue: yes
    harden-dnssec-stripped: yes
    harden-below-nxdomain: yes
    harden-referral-path: yes
    hide-identity: yes
    hide-version: yes

    # Performance
    prefetch: yes
    prefetch-key: yes
    minimal-responses: yes
    rrset-roundrobin: yes

    # Privacy
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10

forward-zone:
    name: "."
    forward-tls-upstream: yes
    forward-addr: 9.9.9.9@853#dns.quad9.net
```

### 3. Pi-hole Configuration

1. Settings → DNS
2. Uncheck all upstream DNS
3. Set Custom DNS: `127.0.0.1#5335`

### 4. Apply Changes

```bash
sudo systemctl restart unbound
pihole restartdns
```

Issue: 'curl -o /var/lib/unbound/root.hints https://www.internic.net/domain/named.root'
