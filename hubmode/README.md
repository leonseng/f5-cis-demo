# Hub mode

## Observed behaviour
- namespace label
- how to stop someone else from hijacking the configmap with labels? - once a pool has discovered endpoints, another service cannot introduce its own - endpoints into the pool
Can 2 applications take the same label? - no, first come first serve
- each tenancy should have its own AS3 configmap. all applications in same tenancy must be defined in the same configmap as AS3 is scoped by tenancy. If each application has its own AS3 configmap (in the same tenancy), the last one takes effect.
- multiple hubmode AS3 supported
- namespace-label is only used to monitor where AS3 configmap is defined. Services can be in any namespace (without label) as CIS has clusterrole (probably)

## Best practice
cluster admin define hubmode AS3 configmaps in their own namespace (labeled use_cis=true) to control exposure on BIG-IP, devs just create services and apply labels provided to them