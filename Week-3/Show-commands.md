# MLS1 Verification Commands

## Connectivity Check (VLAN10 → R1 Loopback)
```cisco
show ip ospf neighbor
show ip route 8.8.8.8
ping 8.8.8.8
```

## QoS Configuration
```cisco
show policy-map MLS-QOS
show class-map
```

## QoS Active on Interfaces
```cisco
show policy-map int gi1/0/1
show policy-map int gi1/0/2
```

## Traffic Generation & Monitoring
Start webserver traffic generator, then run:
```cisco
show policy-map int gi1/0/1
show policy-map int gi1/0/2
show access-lists WEB_SERVER
```

## Expected Results
| Class | Limit | Expected Drops |
|-------|-------|----------------|
| VOICE_TRAFFIC | 40 Mbps | When > 40 Mbps |
| WEB_SERVER | 20 Mbps | When > 20 Mbps |
| class-default | 2 Mbps | When > 2 Mbps |