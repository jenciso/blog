---
title: Recovery admin password for grafana
linktitle: recovery-admin-password-for-grafana
comments: true
date: 2018-01-13
categories:
  - grafana
---
## Steps to recovery admin password for grafana 

Install sqlite3

```sh
sudo apt-get install sqlite3
```
 
Login to the database

```sh
sudo sqlite3 /var/lib/grafana/grafana.db
```
 
Reset the admin password to "admin"

```sh
sqlite> update user set password = '59acf18b94d7eb0694c61e60ce44c110c7a683ac6a8f09580d626f90f4a242000746579358d77dd9e570e83fa24faa88a8a6', salt = 'F3FAxVm33R' where login = 'admin';
sqlite> .exit
```  

Now you can login using this credentials: `username: admin` and `password: admin`
 

Obviusly, afterward you need to change the admin password.
