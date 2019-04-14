# SQL2016_UnattendedInstall_Ansible
SQL Server 2016 unattended install on vCenter using Ansible

Just a few playbooks to create a new VM and launch an unattended install of SQL Server 2016. The purpose was to apply
typical VMware best practices for virtualized SQL Server workloads, notably:
- separate vdisks for SQL log, tempdb, and data volumes
- separate PVSCSI controllers for SQL log, tempdb and data volumes (remainder of volumes share the remaining controller)
- registry settings for PVSCSI buffer increase

There was a pull request by Abhijeet Kasurde (@akasurde) <akasurde@redhat.com> to add vmware_guest_disk.py, which addresses
the fact the vmware_guest doesn't include options to specify PVSCSI controller IDs. While the latest vmware_guest does
have the option to specify whether the new disk is independent-persistent, etc...I had to modify Abhijeet's module to 
do this and apply the best practice. Making the disks independent-persistent will make sure they don't get caught up in VADP 
backups, which tend to fail on high IO disks such as SQL data, tempdb or log. Just didn't have time to submit my suggested
changes....
