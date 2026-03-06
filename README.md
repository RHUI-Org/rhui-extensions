# RHUI Extensions
Just a bunch of tips, tricks and scripts that RHUI administrator might find useful.

## Repository labels and ids for different RHEL products
### IMPORTANT !!!
This repo contains the best EFFORT to provide lists of repositories and repo ids that you need to provide working RHEL clients for your RHEL cloud images.
It is therefor quite possible - even probable - that it will be updated over time as new requirements are implemented. If unsure, contact your TAM.

### RHUI 4 vs RHUI 5 NOTE
While RHUI 4 and 5 differ significantly in the way they are deployed - RHUI4 runs directly on metal, RHUI5 runs in container - functionality and commands remain the same.
However if you are on RHUA node in RHUI 5, you first need to get into container using:
```bash
rhua
```
and then run the commands.
Alternatively run each of the commands with prefix:
```bash
rhua rhui-manager --noninteractive repo sync_all
```

### How to use
In content/ directory you can find script **generate_commands** that takes one YAML file as a parameter.
In subdirectories named after RHEL versions you can find a number of those YAML files each providing lists of repositories and repo labels necessary for successful creation of working RHEL client and adding of necessary repositories into RHUI.


#### EXAMPLE:
1. Go to **content/** directory
2. Run:
```bash
./generate_commands rhel8/rhel8.yaml
```

3. Output should be:

```text
Warning: There is no rhui-extensions.conf in this directory or in ~/ directory. Consider creating one based on rhui-extensions.conf-example from repository root.
rhui-manager --noninteractive repo add_by_file --file rhel8/rhel8.yaml

rhui-manager --username admin --password admin client cert --name rhel8 --days 730 --dir /tmp --repo_label rhel-8-for-x86_64-appstream-beta-rhui-rpms,rhel-8-for-aarch64-supplementary-eus-rhui-rpms,codeready-builder-for-rhel-8-aarch64-eus-rhui-rpms,rhel-8-for-x86_64-supplementary-eus-rhui-rpms,rhel-8-for-aarch64-baseos-beta-rhui-rpms,rhel-8-for-aarch64-supplementary-rhui-rpms,rhel-8-for-aarch64-supplementary-beta-rhui-rpms,codeready-builder-for-rhel-8-x86_64-eus-rhui-rpms,rhel-8-for-x86_64-appstream-rhui-rpms,codeready-builder-for-rhel-8-aarch64-rhui-rpms,rhel-8-for-x86_64-appstream-eus-rhui-rpms,rhel-8-for-aarch64-appstream-rhui-rpms,rhel-8-for-aarch64-baseos-rhui-rpms,rhel-8-for-aarch64-appstream-eus-rhui-rpms,rhel-8-for-aarch64-baseos-eus-rhui-rpms,rhel-8-for-aarch64-appstream-beta-rhui-rpms,rhel-8-for-x86_64-baseos-beta-rhui-rpms,codeready-builder-for-rhel-8-x86_64-rhui-rpms,rhel-8-for-x86_64-baseos-eus-rhui-rpms,ansible-2-for-rhel-8-aarch64-rhui-rpms,ansible-2-for-rhel-8-x86_64-rhui-rpms,rhel-8-for-x86_64-supplementary-rhui-rpms,rhel-8-for-x86_64-baseos-rhui-rpms

rhui-manager --username admin --password admin client rpm --private_key rhel8.key --entitlement_cert rhel8.crt --dir /tmp --rpm_name rhel8

# Syncing of ALL repos
# !!! NOTE !!! If you plan to add multiple RHEL versions/variants, run this step only after you add all repos.
rhui-manager --username admin --password admin repo sync_all
```

## rhui-extensions.conf
You might have noticed warning in previous example output. Generate command will point you to **rhui-extensions.conf-example** file that you can find right next to this README.md file - we suggest you use it.
Just rename it to **rhui-extensions.conf**, change value for expiration inside and place it into your user's directory.
From that moment generated command for certificate will be tuned for your needs and you can use it as cut and paste.