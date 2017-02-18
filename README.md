# erpnext

Erpnext docker image, the project is forked from [davidgu/erpnext](https://github.com/pfy/erpnext)

* Based on: ubuntu:trusty
* Including services: 
  * Redis
  * Nginx
  * memcached
  * Maridb
  * cron

## run data container

```bash
$ docker create -v /home/frappe/frappe-bench/sites/site1.local/ -v /var/lib/mysql --name erpdata cloudhsiao/erpnext
```

## run erpnext

```bash
$ docker run -d -p 80:80 -p 8000:8000 --name erpnext --volumes-from erpdata cloudhsiao/erpnext
```

Expose port 8000 if you wanna use dev mode, or you can use `-P` instead.

```bash
$ docker run -d -P --name erpnext --volumes-from erpdata cloudhsiao/erpnext
```

## get passwords

```bash
$ docker exec -ti erpnext cat /root/frappe_passwords.txt
```

Login on http://localhost with Administrator / password

## switch to dev mode

Open shell of running container by entering:

```bash
$ docker exec -it erpnext /bin/bash
```

And then enter the following in the container.

```bash
cd frappe-bench
rm config/supervisor.conf
rm config/nginx.conf
service nginx stop
service supervisor stop
```

The container will stop after stopping supervisor, you should re-start and open the shell.

```bash
$ docker start erpnext
$ docker exec -it erpnext /bin/bash
```

Restart the bench in the container.

```bash
bench start
```

Finally, detach from the container by pressing `ctrl + p` and `ctrl + q`.
