# Config Map mode

Cluster admin defines Kubernetes ConfigMaps which contains an F5 AS3 template. To link a Kubernetes service to the AS3 ConfigMap, application developers need to label their service as such:

```
apiVersion: v1
kind: Service
metadata:
  labels:
    cis.f5.com/as3-tenant: <as3_tenancy>
    cis.f5.com/as3-app: <as3_app>
    cis.f5.com/as3-pool: <as3_pool>
    ...
```

CIS performs endpoint discovery of the Kubernetes service, combines it with the AS3 ConfigMap, and configures BIG-IP with the relevant configuration in the specified tenant/partition.

> Note: these tenants/partitions are not the same as the one specified by the `--bigip-partition` option. They are managed separately.

## Hub Mode
By default, CIS only process AS3 ConfigMaps and Services in the same namespace, which works if the application developers and cluster administrators are the same team. Otherwise, it's better to set `--hubmode=true` so that the AS3 ConfigMaps can be defined in secure namespaces. When used with the `--namespace` or `--namespace-label` options, application developers will be prevented from modifying the BIG-IP configuration.

## Observed behaviour
- `--namespace-label` only determines which namespace the AS3 ConfigMaps should be in. Namespaces where the applications and Services are located do not require the label, as CIS will have cluster wide privileges (unless restricted otherwise with RoleBindings) to discover the Services and their endpoints
- Once a pool has been assigned to a Service, a new Service using same label will not its endpoint added to the pool. In Hub Mode, there is a risk of one application team using another team's label, "hijacking" the other team's pool on BIG-IP.
- As AS3 is scoped by tenant/partition, each tenant should have its own AS3 configmap, containing all application/virtual server definitions in it. Splitting a tenant into multiple ConfigMaps (one for each application in the tenant) will cause the last ConfigMap to take effect, removing configurations of all other applications.

---

## Troubleshoot

### CIS not processing Custom Resources

Check to ensure CIS is configured to monitor the namespace in which the Custom Resources are defined. If `--namespace-label` is specified, ensure the namespace is labelled appropriately

```
kubectl label namespace <namespace> <key>=<value>
```

### Demo pods crashing

When deploying the demo in non-default namespace, the pods may fail to run due to OCP's Security Context Constraints (SCC). If so, add the serviceAccount in the namespace to the `anyuid` SCC.

```
oc adm policy add-scc-to-user anyuid system:serviceaccount:<namespace>:<serviceAccount>
```
