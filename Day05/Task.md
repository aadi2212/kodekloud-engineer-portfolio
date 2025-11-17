✅ Day 5: SELinux Installation and Configuration
✅ 1. Task Overview

Enhance server security on App Server 2 by installing SELinux management packages and configuring SELinux. For testing purposes, SELinux will be temporarily disabled and re-enabled after necessary configuration changes. No immediate reboot is required.

✅ 2. Prerequisites

Access to App Server 2 via a user with sudo privileges.

Basic knowledge of SELinux and Linux package management (yum).

✅ 3. Step-by-Step Instructions

Install SELinux packages

sudo yum install -y policycoreutils selinux-policy selinux-policy-targeted


Permanently disable SELinux

Edit the SELinux configuration file to disable it permanently:

sudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config


Alternative: Manually edit /etc/selinux/config and set:

SELINUX=disabled


Temporarily disable SELinux without reboot

sudo setenforce 0


This sets SELinux to permissive mode until the server is rebooted.

Verify SELinux status

getenforce


Expected Output:

Permissive


After the planned maintenance reboot, SELinux should be disabled.

✅ 4. Verification

Run getenforce to confirm temporary permissive mode.

Check /etc/selinux/config to ensure SELINUX=disabled is set for permanent disablement.

✅ 5. Conclusion

SELinux management packages were installed, SELinux was temporarily disabled for testing, and permanent disablement was configured for after the planned reboot. This setup prepares the server for future SELinux-based security enhancements while maintaining operational continuity.


