# Create a backup & restore user
1.	Create a **backup_role** with below cluster privileges
```
*Monitor 
manage_slm 
cluster:admin/snapshot 
cluster:admin/repository
```
# and with below Index privileges
```
Indices		Privileges
monitor 	all
```
2.	Create a user **backup_user** with **backup-role**.  ## User can be created using Kibana 
