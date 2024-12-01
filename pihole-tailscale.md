# EC2 and Pi-hole Configuration

## EC2 Instance

- **Type**: t3.small (2 vCPU, 2GB RAM)
- **OS**: Ubuntu 22.04 LTS
- **Storage**: 8GB gp3 EBS
- **Network Security Group**:
  - Inbound:
    - UDP 41641 (Tailscale)
    - TCP/UDP 53 (DNS)
    - TCP 22 (SSH)
  - Outbound:
    - All traffic (required for updates and DNS resolution)

## Pi-hole Setup

- **Interface**: tailscale0
- **DNS**: Quad9 via Unbound
  - Encrypted DNS (TLS)
  - No logging
  - DNSSEC enabled
- **Security**:
  - Listen on tailscale0 only
  - Query logging disabled
  - DNS rebinding protection
