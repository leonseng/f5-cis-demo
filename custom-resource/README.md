# Custom Resource mode

Enabling [Custom Resource mode](https://clouddocs.f5.com/containers/latest/userguide/crd/) replaces the other modes, i.e. CIS will not process Ingress/Routes/ConfigMaps.

**Pros**
- Using CR mode allows better access control of BIG-IP configuration, as definition of the custom resources can be restricted to cluster admins only.
- Supports dynamic IP allocation on VirtualServer and K8s loadbalancer Service through F5 IPAM

**Cons**
- VirtualServer CR is not very customizable, e.g. can't attach iRule as of 2.5.1

---

## Instructions

Setup
```
oc apply -f cis_crmode_deploy.yaml
oc apply -f demo/
```

Test
```
curl http://multi.example.com/f5
curl http://multi.example.com/nginx
```

Clean up
```
oc delete -f cis_crmode_deploy.yaml
oc delete -f demo/
```

---

## Troubleshoot

### CIS not processing Custom Resources

Check to ensure CIS is configured to monitor the namespace in which the Custom Resources are defined. If `--namespace-label` is specified, ensure the namespace is labelled appropriately

```
kubectl label namespace <namespace> <key>=<value>
```
