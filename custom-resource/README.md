# Custom Resource mode

Enabling Custom Resource mode replaces the other modes, i.e. CIS will not process Ingress/Routes/ConfigMaps.

**Pros**
- Using CR mode allows better access control of BIG-IP configuration, as definition of the custom resources can be restricted to cluster admins only.
- Supports dynamic IP allocation on VirtualServer and K8s loadbalancer Service through F5 IPAM

**Cons**
- VirtualServer CR is not very customizable, e.g. can't attach iRule as of 2.5.1
