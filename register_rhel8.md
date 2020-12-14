```
[root@RHEL8-3 ~]# yum repolist
Updating Subscription Management repositories.
Unable to read consumer identity
This system is not registered to Red Hat subscription Management. You can use subscription-manager to register.
No repositories available```

`[root@RHEL8-3 ~]# subscription-manager register 
Registering to: subscription.rhsm.redhat.com:443/subscription
Username: username
Password: 
The system has been registered with ID: 1918d36e-6857-40ba-ab06-e95396f80266
The registered system name is: RHEL8-3.localdomain`

[root@RHEL8-3 ~]# subscription-manager attach --auto
Installed Product Current Status:
Product Name: Red Hat Enterprise Linux for x86_64
Status:       Subscribed

[root@RHEL8-3 ~]# subscription-manager list --available
+-------------------------------------------+
    Available Subscriptions
+-------------------------------------------+
Subscription Name:   30 Day Red Hat Enterprise Linux Server Self-Supported Evaluation
Provides:            Red Hat Beta
                     Oracle Java (for RHEL Server)
                     Red Hat Enterprise Linux Server
                     Red Hat CodeReady Linux Builder for x86_64
                     Red Hat Enterprise Linux for x86_64
                     Red Hat Ansible Engine
                     Red Hat Container Images Beta
                     Red Hat Enterprise Linux Atomic Host Beta
                     Red Hat Enterprise Linux Atomic Host
                     Red Hat Container Images
SKU:                 RH00065
Contract:            12530278
Pool ID:             8a85f99a75fb07cc017628bb2743749d
Provides Management: No
Available:           1
Suggested:           0
Service Type:        L1-L3
Roles:               Red Hat Enterprise Linux Server
Service Level:       Self-Support
Usage:               Development/Test
Add-ons:             
Subscription Type:   Instance Based
Starts:              12/03/2020
Ends:                01/02/2021
Entitlement Type:    Physical

[root@RHEL8-3 ~]# subscription-manager attach --pool=8a85f99a75fb07cc017628bb2743749d
Successfully attached a subscription for: 30 Day Red Hat Enterprise Linux Server Self-Supported Evaluation


[root@RHEL8-3 ~]# subscription-manager list
+-------------------------------------------+
    Installed Product Status
+-------------------------------------------+
Product Name:   Red Hat Enterprise Linux for x86_64
Product ID:     479
Version:        8.3
Arch:           x86_64
Status:         Subscribed
Status Details: 
Starts:         12/03/2020
Ends:           01/02/2021

[root@RHEL8-3 ~]# yum repolist
Updating Subscription Management repositories.
repo id                                                                  repo name
rhel-8-for-x86_64-appstream-rpms                                         Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)
rhel-8-for-x86_64-baseos-rpms                                            Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)
[root@RHEL8-3 ~]# subscription-manager repos --list
+----------------------------------------------------------+
    Available Repositories in /etc/yum.repos.d/redhat.repo
+----------------------------------------------------------+
Repo ID:   ansible-2-for-rhel-8-x86_64-debug-rpms
Repo Name: Red Hat Ansible Engine 2 for RHEL 8 x86_64 (Debug RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/layered/rhel8/x86_64/ansible/2/debug
Enabled:   0

Repo ID:   rhel-8-for-x86_64-baseos-source-rpms
Repo Name: Red Hat Enterprise Linux 8 for x86_64 - BaseOS (Source RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel8/$releasever/x86_64/baseos/source/SRPMS
Enabled:   0

Repo ID:   codeready-builder-for-rhel-8-x86_64-debug-rpms
Repo Name: Red Hat CodeReady Linux Builder for RHEL 8 x86_64 (Debug RPMs)
Repo URL:  https://cdn.redhat.com/content/dist/rhel8/$releasever/x86_64/codeready-builder/debug
Enabled:   0


[root@RHEL8-3 ~]# subscription-manager repos --enable=codeready-builder-for-rhel-8-x86_64-rpms
Repository 'codeready-builder-for-rhel-8-x86_64-rpms' is enabled for this system.

[root@RHEL8-3 ~]# subscription-manager repos --enable=rhel-8-for-x86_64-appstream-rpms
Repository 'rhel-8-for-x86_64-appstream-rpms' is enabled for this system.

[root@RHEL8-3 ~]# yum repolist
Updating Subscription Management repositories.
repo id                                                                      repo name
codeready-builder-for-rhel-8-x86_64-rpms                                     Red Hat CodeReady Linux Builder for RHEL 8 x86_64 (RPMs)
rhel-8-for-x86_64-appstream-rpms                                             Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)
rhel-8-for-x86_64-baseos-rpms                                                Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)


[root@RHEL8-3 ~]# yum update -y
