```[root@RHEL8-3 ~]# yum repolist
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
