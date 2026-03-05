# Building of Custom Client RPM

Following text is based on a part of knowledge base article found here: https://access.redhat.com/articles/4070201

## Requirements
1. You will need **rpmbuild** package installed. It's standard part of RHEL or Fedora.

## Building a Client RPM

After you run a command from **generate_commands** - for example:
```bash
rhui-manager --username admin --password superpass client rpm --private_key rhel8.key --entitlement_cert rhel8.crt --dir /tmp --rpm_name rhel8
```

You should see a line like:
```bash
Location: /tmp/rhel8-2.0/build/RPMS/noarch/rhel8-2.0-1.noarch.rpm
```

The only problem with this client package is that in its rh-cloud.repo file there will be a lot of hardcoded stuff:
1. There will be FQDN of RHUI that created it.
2. Architecture variants will be hardcoded.
3. All repositories will be **enabled=1**.


## Update BaseArch and Disable Sources/Debug Repos

Copy core stuff to work directory:
```bash
mkdir ~/rpmbuild
cp -r /tmp/rhel8-2.0/build/SPECS/ ~/rpmbuild/
cp -r /tmp/rhel8-2.0/build/SOURCES/ ~/rpmbuild/
```

Extract it:
```bash
cd ~/rpmbuild/SOURCES/
gunzip rhel8-2.0.tar.gz
tar xf rhel8-2.0.tar
```

Now edit the rh-cloud.repo file:
```bash
vi rhel8-client-2.0/rh-cloud.repo
```

If you have repositories for multiple architectures available, we suggest you use the $basearch variable in the URL; it can also be used in the name field.

Original:
```bash
name=Red Hat Enterprise Linux 8 for ARM 64 - AppStream (Debug RPMs) from RHUI
mirrorlist=https://rhui.example.com/pulp/mirror/content/dist/rhel8/rhui/8/aarch64/appstream/debug
```

Modified:
```bash
name=Red Hat Enterprise Linux 8 for $basearch - AppStream (Debug RPMs) from RHUI
mirrorlist=https://rhui.example.com/pulp/mirror/content/dist/rhel8/rhui/8/$basearch/appstream/debug
```

At this moment you can and should **disable source and debug variants of the repos** as most of the user won't need those and it will slow down dnf operations as there will be big metadata files that dnf will need to download.
Locate
```bash
enabled=1
```
lines of the Debug and Source repositories and change those.

### NOTE
There are other repos that we generally suggest to be disabled by default:
1. Ansible 2 on RHEL 8
2. CodeReady repos
3. RHEL Extensions
4. Supplementary
5. Optional
6. Extra
But it depends on user's use cases and your discression.


## Adding $releasever For Numbered Variants
There is one more variable that is useful: **$releasever**. It denotes the version of the RHEL release you want to use. It does not make sense in previous example, but you can use it to prepare a single client for all EUS releases of, for example, RHEL 9. There is a default script **rhui-set-release** attached to generated client that changes $releasever and enables users to change EUS versions on a client.
To use it just replace hardcoded version into variable. For example:
Original:
```bash
name=Red Hat Enterprise Linux 8 for ARM 64 - AppStream (Debug RPMs) from RHUI
mirrorlist=https://rhui.example.com/pulp/mirror/content/eus/rhel8/rhui/8.8/aarch64/appstream/debug
```
into:
```bash
name=Red Hat Enterprise Linux 8 for $basearch - AppStream (Debug RPMs) from RHUI
mirrorlist=https://rhui.example.com/pulp/mirror/content/eus/rhel8/rhui/$releasever/$basearch/appstream/debug
```


## Rebuilding of the client
Now that you have your new version of the rh-cloud.repo file, re-pack it and build the final RPM:
```bash
tar cfz rhel8-2.0.tar.gz rhel8-2.0/
cd ../SPECS
rpmbuild -ba rhel8-2.0.spec
```

Your new client RPM will be located at **~/rpmbuild/RPMS/noarch/rhel8-2.0-1.noarch.rpm**.
