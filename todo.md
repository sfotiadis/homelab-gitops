# External DNS + Ingress Setup Requirements

**Context:** This homelab Kubernetes cluster uses HAProxy as external load balancer (not MetalLB/LoadBalancer IPs). ExternalDNS must work with this architecture.

## Infrastructure Overview

### Network Architecture
```
Internet/VPN → HAProxy (192.168.34.135) → Worker Nodes (NodePort 30080/30443) → Ingress Controller → Apps
                                        ↓
                              ExternalDNS → BIND9 DNS (192.168.34.2/3)
```

### Key Components
- **HAProxy**: Single entry point at `192.168.34.135` (k8s-lb-01.crulabs.intern)
  - Port 80 → Workers NodePort 30080
  - Port 443 → Workers NodePort 30443
- **DNS**: BIND9 Primary (192.168.34.2) + Secondary (192.168.34.3)
  - Zones: `crulabs.intern`, `internal`
  - RFC2136 dynamic updates enabled
  - TSIG key: `/etc/bind/externaldns.key` on dns1
- **Workers**: 192.168.34.140-142 (all have NodePorts exposed)

## Critical Configuration Points

### 1. Ingress Controller Deployment
**MUST use NodePort, NOT LoadBalancer type:**
```yaml
service:
  type: NodePort
  nodePorts:
    http: 30080
    https: 30443
```

**Why:** No MetalLB/LoadBalancer provider. HAProxy routes to these NodePorts.

### 2. ExternalDNS Configuration

**Target for DNS records:**
```yaml
args:
  - --source=ingress
  - --provider=rfc2136
  - --rfc2136-host=192.168.34.2  # Primary DNS
  - --rfc2136-zone=crulabs.intern
  - --domain-filter=crulabs.intern
  - --txt-owner-id=crulabs-k8s
  - --policy=sync
```

**Ingress annotation strategy:**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    external-dns.alpha.kubernetes.io/target: "k8s-lb-01.crulabs.intern"  # HAProxy CNAME
spec:
  rules:
  - host: myapp.crulabs.intern
```

**Result:** `myapp.crulabs.intern` → CNAME → `k8s-lb-01.crulabs.intern` → A → `192.168.34.135`

### 3. TSIG Secret for ExternalDNS

**Extract from dns1:**
```bash
ssh ubuntu@192.168.34.2
sudo cat /etc/bind/externaldns.key
# Copy the base64 secret value
```

**Create Kubernetes Secret:**
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: external-dns-tsig
  namespace: external-dns
stringData:
  rfc2136_tsig_secret: "<base64-secret-from-bind>"
  rfc2136_tsig_keyname: "externaldns-key"
  rfc2136_tsig_algorithm: "hmac-sha256"
```

### 4. DNS Server Configuration (Already Done)

BIND9 allows dynamic updates:
```bind
zone "crulabs.intern" {
    type master;
    file "/var/lib/bind/db.crulabs.intern";
    allow-update { key "externaldns-key"; };
    notify yes;
};
```

## Deployment Order

1. ✅ **Infrastructure** (already done)
   - HAProxy configured for ports 80/443
   - DNS servers with RFC2136 enabled
   - TSIG key generated

2. **Ingress Controller** (your task)
   - Deploy with NodePort 30080/30443
   - Verify pods running on workers

3. **ExternalDNS** (your task)
   - Create TSIG secret
   - Deploy ExternalDNS with RFC2136 provider
   - Configure to use CNAME target annotation

4. **Test Ingress** (your task)
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: test-app
     annotations:
       external-dns.alpha.kubernetes.io/target: "k8s-lb-01.crulabs.intern"
   spec:
     ingressClassName: nginx  # or your ingress class
     rules:
     - host: test.crulabs.intern
       http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: test-app
               port:
                 number: 80
   ```

## Validation Commands

```bash
# Check Ingress Controller
kubectl get svc -n ingress-nginx  # Should show NodePort

# Check ExternalDNS
kubectl logs -n external-dns -l app=external-dns

# Verify DNS record created
dig @192.168.34.2 test.crulabs.intern

# Test from outside cluster
curl http://test.crulabs.intern
```

## Common Pitfalls

❌ **Don't:** Use LoadBalancer service type (no MetalLB)
✅ **Do:** Use NodePort 30080/30443

❌ **Don't:** Point ExternalDNS directly to worker IPs
✅ **Do:** Use CNAME to HAProxy hostname

❌ **Don't:** Use `--source=service` with type LoadBalancer
✅ **Do:** Use `--source=ingress` with target annotation

❌ **Don't:** Forget TSIG secret
✅ **Do:** Create secret before deploying ExternalDNS

## Architecture Rationale

**Why HAProxy + NodePort instead of MetalLB?**
- Works across VLANs/VPNs (no L2 dependency)
- Single entry point for troubleshooting
- Already in place for K8s API HA
- Simple, battle-tested approach

**Why CNAME to HAProxy instead of direct A records?**
- Single source of truth (HAProxy IP in BIND9)
- Easy to change HAProxy IP (one DNS record)
- Clear traffic flow documentation

## Reference

- BIND9 config: `ansible/templates/bind9/`
- HAProxy config: `ansible/templates/haproxy/haproxy.cfg.j2`
- FluxCD setup: `ansible/playbooks/06-fluxcd-bootstrap.yml`
- SOPS GPG fingerprint: `2F45BC42327907C5B833A93041413E5C18474DDB`

**All infrastructure automation is in the `homelab-automation` repository. Application deployments belong in `homelab-gitops` with FluxCD.**
