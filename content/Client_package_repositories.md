# Client package repositories

It is generally a good idea to create a client package that will also contain the path to a special custom repository where the client package itself is located.

## Example: How to create a RHEL 8 SAP Client repository

1. First create a custom repository on your RHUI:
```bash
rhui-manager repo create_custom --protected --repo_id rhui-client-config-8-sap --display_name "RHUI Client Configuration RHEL 8 SAP" --path rhui-client-config/rhel/server/8/x86_64/sap
```

2. When creating any SAP client package, include this repository so that its path is in the certificate.
3. Upload the client packages for SAP into this repository:
```bash
rhui-manager packages upload --repo_id rhui-client-config-8-sap --packages /root/sap8-1.0.0.rpm
```

## Why?

1. You now have a simple way to distribute updates to client packages if needed — every SAP client will be able to see if there is a newer version of itself.
2. If you place all streams into the same repository — e.g., E4S, EUS, and dist — it is much easier for end users to do version upgrades and migrations, as a RHEL 8.8 E4S SAP client can actually download a RHEL 8.10 SAP client.