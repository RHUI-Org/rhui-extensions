# LeApp In-place Upgrades Of RHEL in RHUI Environment
By default, LeApp updates expect machines to be registered to the Red Hat CDN using subscription-manager.
While it is possible to perform LeApp updates even in disconnected RHUI environments, it might be quite challenging to set up.
This guide is our best effort to point RHUI administrators toward the right documentation.

## How to create custom LeApp client
The LeApp package available in RHEL repositories is delivered with support for updates of machines registered to the Red Hat CDN and those that run in AWS, Azure, and GCP.

If you want to have LeApp working in your cloud environment, you need to create a custom variant. Here's the knowledge base article that discusses how to do it:
* https://access.redhat.com/articles/7115872

## List of supported upgrade paths
LeApp updates are supported only from certain versions to certain versions of RHEL. As a general rule, it is always the latest version of RHEL to the latest version of RHEL one major version higher.

The complete list of those supported paths can be found in the documentation for the particular RHEL version. Each documentation describes the update from one RHEL version below:
* For RHEL 8 it describes the update from RHEL 7 - https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/upgrading_from_rhel_7_to_rhel_8/con_supported-upgrade-paths-rhel-7-to-rhel-8_upgrading-from-rhel-7-to-rhel-8
* Update from RHEL 8 to RHEL 9 - https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/upgrading_from_rhel_8_to_rhel_9/con_supported-upgrade-paths_upgrading-from-rhel-8-to-rhel-9
* Update from RHEL 9 to RHEL 10 - https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/upgrading_from_rhel_9_to_rhel_10/supported-upgrade-paths

Note that for updates to the current version of RHEL, the version numbers are updated every time a new version of RHEL is released. So the supported path at the moment of writing is RHEL 9.7 to RHEL 10.1, but once RHEL 9.8 and 10.2 are released, it will be updated to the newest versions. As RHEL 9 reaches its final version (9.10), the target RHEL 10 version will be updated every time a new version of RHEL 10 is released.

## How to use repo listings
What LeApp clients need is a certificate that provides access to content for both versions of RHEL. That is why there are two different RHEL versions in the `repo_labels` section of the listings. However, you really only need to make sure that you have the correct target repository version, which is why those listings contain only target RHEL-numbered repositories.

There are also supported update paths for SAP and EUS versions, but our LeApp listings do not specify those, as the size of the lists would grow too much. The provided repositories are really those that will most likely be missing in your RHUI — those numbered "dist" repositories.

If you want to create a LeApp client for EUS or SAP, just pick both the source and target versions, locate them in the other EUS/SAP listings, and join them together to create the certificate needed by LeApp. You should already have the repositories added/synchronized.