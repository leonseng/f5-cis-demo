# OCP Route

CIS configures a pair of virtual servers on BIG-IP (port 80 and 443) in the CIS managed partition (specified by the `--bigip-partition` option). For every Openshift route configured, CIS creates and attaches a policy to the virtual server that filters by hostnames, and routes to the corresponding pool member

The virtual servers defined can be customized further using the Override AS3 ConfigMap.

---

## Instructions

Setup
```
oc apply -f cis_deploy.yaml
oc apply -f demo/
```

Test
```
while true; do curl -s mysite.f5demo.com/txt | grep "Server IP"; sleep 1; done
```

Clean up
```
oc delete -f cis_deploy.yaml
oc delete -f demo/
```
