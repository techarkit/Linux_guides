 ## How to Create New User in RHV-M GUI Access
 
 - Create User
 `ovirt-aaa-jdbc-tool user add techarkit --attribute=firstName=RaviKumar`
 
 - Reset Password
 `ovirt-aaa-jdbc-tool user password-reset techarkit --password=pass:Redh@t`
 
 - Unlock User
 `ovirt-aaa-jdbc-tool user unlock techarkit`
 
 - Check User Details
 `ovirt-aaa-jdbc-tool user show techarkit`
 
 - Change Expiry date of the user
 `ovirt-aaa-jdbc-tool user edit techarkit --password-valid-to="2022-12-31 23:30:53Z"`
 
 - Check the User Details
 `ovirt-aaa-jdbc-tool user show techarkit`
