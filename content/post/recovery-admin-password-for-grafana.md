---
title: Recovery admin password for grafana
linktitle: recovery-admin-password-for-grafana
thumbnail: https://www.hostinger.com/blog/wp-content/uploads/sites/4/2017/11/grafana-logo.jpg
comments: true
date: 2018-01-13
categories:
  - grafana
---

## Intro 

Sometimes you need to enter in your grafana system, but for some reason, you lost your admin password. If are you using sqlite as backend storage? Then following these steps:

* In your local machine, install the sqlite3 package

{{< highlight sh >}}
sudo apt-get install sqlite3
{{< /highlight >}}
 
* Login to the database

{{< highlight sh >}}
sudo sqlite3 /var/lib/grafana/grafana.db
{{< /highlight >}}
 
* Reset the admin password to "admin"

{{< highlight sql >}}

sqlite> update user set password = '59acf18b94d7eb0694c61e60ce44c110c7a683ac6a8f09580d626f90f4a242000746579358d77dd9e570e83fa24faa88a8a6', salt = 'F3FAxVm33R' where login = 'admin';
sqlite> .exit

{{< /highlight >}}

Now you can login using this credentials: `username: admin` and `password: admin`
 

Of course, the final step will be, change your grafana environment for another admin password
