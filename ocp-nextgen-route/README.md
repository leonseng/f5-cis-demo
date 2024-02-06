# CIS NextGen Controller

## Setup
```
helm install -f values.yaml cis f5-stable/f5-bigip-ctlr
```

# Notes
- "Top level" extended spec must be defined in namespace watched by CIS.
- In the top level extended spec, namespace route may have the `allowOverride` flag set to true, allowing a child extended spec to be defined within the app namespace for app teams to override.
