# How to migrate between SAP versions

Following guide is really only a general explanation of a problem and how to approach it. Based on CCSP's offerings source and target products might differ.

For example there might be SAP Applications offering, Update Services for SAP Solutions, SAP Hana, different streams like EUS and E4S. Doing complete path for each combination is not viable.

## Note on RHEL 8.10 SAP
During the lifecycle of RHEL, the SAP product is usually distributed in EUS or E4S streams - generally every even minor version. However once RHEL major release get to it's final version - RHEL 8.10, RHEL 9.10, RHEL 10.10 - there are no longer EUS/E4S variants of that version - it is final version after all and it will be supported for couple more years than usual minor version - in that sense it behaves as EUS stream.

That creates a upgrade problem, however. When upgrading to this final major version client machine needs to swap it's stream.


## General Approach to upgrades
Regardless of what products you (CCSP) are offering - approach to upgrades/migrations is almost always the same:
1. If you are staying on the same distribution stream (eg. eus,e4s), just make sure that you have synced all available versions of repositories to RHUI. Switch can then be done using standard *rhui-set-release*. Example: Upgrading from RHEL 8.6 SAP E4S to RHEL 8.8 SAP E4S.
2. If your stream differs (eg. your are swaping from RHEL 8.8 SAP EUS to RHEL 8.10 SAP), then you need to make sure that "source" client (RHEL 8.8 SAP EUS) machine has access to "target" client package (RHEL 8.10 SAP). Swaping the client package source machine and running *rhui-set-release* should do. Expect that you will probably need to create some kind of knowledge base article for users - explaining how to download target version client package and also swithing from E4S to dist means that there actually is NOT a release version specified and there is only *8* now - that requires to run *rhui-set-release --unset* after new client package is installed.

## Note on usefullness of client repositories
As you can imagine the point 2. upgrade path can be significantly simplified if both source and target client use the same repository for client packages.
1. You just *dnf download* new package
2. Uninstall old client package
3. Install package downloaded
4. Run *rhui-set-release* (with --unset if you are swaping to RHEL 8.10)