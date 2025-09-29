&nbsp;SElinux installation and configuration 



Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 2 in the Stratos Datacenter:



1\. Install the required SELinux packages.

2\. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.

3\. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

4\. Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.





->





1\. Install SELinux packages



On App Server 2, install the required SELinux management packages (usually policycoreutils, selinux-policy, setools, etc.)



sudo yum install -y policycoreutils selinux-policy selinux-policy-targeted





2\. Permanently disable SELinux



Edit the SELinux configuration file to disable it permanently:



sudo sed -i 's/^SELINUX=.\*/SELINUX=disabled/' /etc/selinux/config



Alternatively, you can manually edit /etc/selinux/config and set:



SELINUX=disabled





3\. Disable SELinux without reboot for now



To immediately disable SELinux (in enforcing or permissive mode), run:



sudo setenforce 0



This command puts SELinux in permissive mode temporarily until reboot.





4] Verify SElinux status

&nbsp; getenforce



You should see Permissive or Disabled



