# How to migrate between SAP versions

The following guide is really only a general explanation of the problem and how to approach it. Based on CCSP's offerings, the source and target products might differ.

For example, there might be SAP Applications offerings, Update Services for SAP Solutions, SAP Hana, and different streams like EUS and E4S. Doing a complete path for each combination is not viable.

## Note on RHEL 8.10 SAP
During the lifecycle of RHEL, the SAP product is usually distributed in EUS or E4S streams - generally every even minor version. However, once a RHEL major release gets to its final version - RHEL 8.10, RHEL 9.10, RHEL 10.10 - there are no longer EUS/E4S variants of that version - it is the final version after all, and it will be supported for a couple more years than a usual minor version. In that sense it behaves as an EUS stream.

That creates an upgrade problem, however. When upgrading to this final major version, the client machine needs to swap its stream.

## General Approach to upgrades
Regardless of what products you (CCSP) are offering, the approach to upgrades/migrations is almost always the same:
1. If you are staying on the same distribution stream (e.g. eus, e4s), just make sure that you have synced all available versions of repositories to RHUI. The switch can then be done using the standard *rhui-set-release*. Example: Upgrading from RHEL 8.6 SAP E4S to RHEL 8.8 SAP E4S.
2. If your stream differs (e.g. you are swapping from RHEL 8.8 SAP EUS to RHEL 8.10 SAP), then you need to make sure that the "source" client (RHEL 8.8 SAP EUS) machine has access to the "target" client package (RHEL 8.10 SAP). Swapping the client package on the source machine and running *rhui-set-release* should do it. Expect that you will probably need to create some kind of knowledge base article for users explaining how to download the target version client package. Also, switching from E4S to dist means that there actually is NOT a release version specified and there is only *8* now - that requires running *rhui-set-release --unset* after the new client package is installed.

## Note on usefulness of client repositories
As you can imagine, the point 2 upgrade path can be significantly simplified if both source and target clients use the same repository for client packages.
1. You just *dnf download* the new package
2. Uninstall the old client package
3. Install the downloaded package
4. Run *rhui-set-release* (with --unset if you are swapping to RHEL 8.10)