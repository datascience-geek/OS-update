# OS-update

OS-Upgrade
RHEL-6 to RHEL-7 Upgrade
Using ISO image
Stages

Check that Red Hat supports the upgrade of your system
Prepare your system for upgrade
Check your system for problems that might affect your upgrade
Upgrade by running the Red Hat Upgrade Tool
Check your support status
Upgrading from Red Hat Enterprise Linux 6 to Red Hat Enterprise Linux 7 is supported only if your system meets the following criteria.

Your system is on the latest version of the Server variant of Red Hat Enterprise Linux 6 for Intel 64 and AMD64 architecture, with all packages up to date. To check, enter the following commands:

# cat /etc/redhat-release
Red Hat Enterprise Linux Server release 6.9 (Santiago)
# arch
x86_64
# yum upgrade -y
Prepare your system for upgrade
Back up all data
Firstly, back up the entire system to avoid potential data loss, and test that your backup works.

Test first
Before you upgrade a production system, you should clone the system and test the upgrade procedure on the clone. This will allow you to prepare for upgrade without risking the production system.

Check system upgrade suitability
Install the Preupgrade Assistant tool
As root, enter the following command to install all the Preupgrade Assistant packages.

yum -y install preupgrade-assistant preupgrade-assistant-el6toel7 redhat-upgrade-tool
Running the Preupgrade Assistant
preupg -v
Viewing results and correcting errors
When you run preupg, detailed results are saved to the /root/preupgrade/result.html You need to check each item for corrections that need to be made before you upgrade.

Upgrade your system
Install the tool
# yum -y install redhat-upgrade-tool
Disable active repositories
# yum -y install yum-utils
# yum-config-manager --disable \*
Perform the upgrade
The upgrade process requires access to Red Hat Enterprise Linux 7 packages. You can specify the location ofan ISO image

# redhat-upgrade-tool --iso iso_path
Some packages that were in the Base package group in Red Hat Enterprise Linux 6 are no longer part of that group in Red Hat Enterprise Linux 7. You may need to configure additional repositories in order to upgrade these packages correctly.

# redhat-upgrade-tool --addrepo optional=http://host name/path/to/repo
Some packages are not reinstalled during the upgrade process because they have no functionally equivalent replacements in Red Hat Enterprise Linux 7. Red Hat does not provide any support for these packages. To remove these packages at the end of the upgrade process, enter the following command:

# redhat-upgrade-tool --cleanup-post
Reboot
When prompted, reboot the system.

Wait for upgrade to complete
After your system reboots, upgrade can take several minutes or several hours, depending on the number of packages to install.

Perform post upgrade tasks
Perform the post upgrade check to validate the system status. If there are any configuration errors, you must fix them before starting services.

Check system status
Check that your system’s subscription details have been updated as part of the upgrade process.

# cat /etc/redhat-release
Red Hat Enterprise Linux Server release 7.4
# yum repolist
Loaded plug-ins: product-id, subscription-manager
repo id    repo name                  status
rhel-7-rpms  Red Hat Enterprise Linux 7 Server (RPMs)   4,323
If the list of your repositories did not update correctly, perform the following commands:

# subscription-manager remove --all
# subscription-manager unregister
# subscription-manager register
# subscription-manager attach --pool=poolID
# subscription-manager repos --enable=repoID
Update all packages
Ensure that all packages are up to date by running the following:

# yum upgrade -y
# reboot
