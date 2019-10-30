---
title: "Host Django Application on AWS"
date: 2019-10-20T11:45:28+08:00
draft: true
---

This is an easy and efficient way to get your Django application hosted on AWS for free. I documented these steps while deploying [Contikey](https://github.com/stanleynguyen/contikey), an application to share articles between friends.

Note: This is NOT meant for production. Rather it is to quickly host an application on AWS such that it can be accessed without running it on your computer. For a more robust deployment, check [this guide](http://michal.karzynski.pl/blog/2013/06/09/django-nginx-gunicorn-virtualenv-supervisor/).

## Creating the database (optional)

We will be using RDS to host our database. Create a MariaDB instance in the RDS console. Choose ap-southeast-1b as the Region.

In the navigation pane, launch a DB Instance and select MariaDB as the engine, with the most supported version (10.0.32).

Check the Publicly Accessible Box, and set Storage Size to 20GB. Choose Yes for Multi-AZ Deployment.
Enter your DB name, DB instance identifier, master username and password.
Leave the other settings as default.

Once the database is created, go to RDS Console > Parameter Groups > Create Parameter Group. Select MariaDB 10.0 as the parameter group family and give it a name and description. Select Create.

In Parameter Groups, select your newly created group and filter for the parameter 'log_bin_trust_function_creators'. Select 'Edit Parameters' and set the value to 1.

Go back to RDS Console > Instances > YourInstance. In the Details pane, note your DB name, username, password, and host endpoint. Remember this for later.

Now to connect to the database, if you have a mysql client such as SequelPro or MySQLWorkbench you can simply input the DB name, username, password, port=3306 and host endpoint into the input fields.

Otherwise, on command line you can run

`mysql -h <host endpoint> -P 3306 -u <username> -p <password>`

Make sure you have MariaDB installed. Otherwise, run

`sudo apt update -y`

`sudo apt install -y mariadb-server`

Start the MariaDB service

`sudo systemctl start mariadb.service`

`sudo systemctl enable mariadb.service`

Secure the installation of MariaDB

`sudo /usr/bin/mysql_secure_installation`

Then login to the MySQL shell:

`mysql -h <host endpoint> -P 3306 -u <username> -p <password>`

You should then be inside the MariaDB monitor. Congrats you are connected to your database!


## Create the server

We will be using EC2 to host our application. Create an EC2 instance. Choose Ubuntu Server 16.04 LTS as the Amazon Machine Image. The free tier is good enough for our needs.

Under security group, make sure to open the ports for SSH (22) and HTTP (80)

Make sure to save the private key given by AWS (we will need it later to login to the server)

Before we configure our ssh settings, lets assign a IP to our AWS instance. By default, AWS will assign any available IP to the instance when it is started. The IP might change is shutdown and started again. We will associate an IP with the instance, which allows us to reserve the IP for that instance.

Go to EC2 Dashboard > Elastic IPs > Allocate new address > Allocate. You will now have a Elastic IP reserved for you use.

Go to Instances, click on your instance. Under Actions > Network > Associate Elastic IP address

At this point, if you have a domain registered you can also point the domain/subdomain to your Elastic IP. Check your domain registrar for more instructions. (I use [Namecheap](https://www.namecheap.com/))

## Connect to the server

Before we connect to the server we need to configure the permission of the private key to prevent it being accessed by other users. Move the private key to the `~/.ssh` folder and execute the following command.

`$ chmod 600 ~/.ssh/your_private_key`

Now to connect to ur server. Go to your command line and type

`$ ssh -i ~/.ssh/your_private_key ubuntu@your_elasticip`

Alternatively create a file named `config` in the `~/.ssh` folder and populate it with the following.

```
Host your_elastic_ip
  Port 22
  IndentityFile ~/.ssh/your_private_key
```
If you have a domain add another record for your domain.

```
Host your_domain
  Port 22
  IndentityFile ~/.ssh/your_private_key
```

Now you can `$ ssh ubuntu@your_elasticip` or `$ ssh ubuntu@your_domain` to connect to your server!


## Set up the server

`$ sudo apt update`

`$ sudo apt upgrade`

Install python packages. `virtualenv` is manage your python environments, while `virtualenvwrapper` (optional) is a really convenient wrapper to easily setup virtual environments.

`$ sudo apt install python-pip`

`$ sudo apt install python3-dev`

`$ sudo apt install libmysqlclient-dev`

`$ sudo pip install virtualenv virtualenvwrapper`

Add the following lines to your `~/.bashrc` file

```
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

And restart it

`$ source ~/.bashrc`

Now create ur virtualenv (assuming python3)

`$ mkvirtualenv --python=/usr/bin/python3 your_app`

## Install the application

We will now install the application and its dependencies

`$ git clone your_app`

`$ cd your_app`

`$ pip install -r path/to/requirements/file`

Test your server (make sure to change ur settings for your database to the one hosted on aws)

`$ python manage.py runserver`

## Route requests

We use nginx to route HTTP requests to our application

Install nginx

`$ sudo apt install nginx`

Configure nginx

`$ sudo rm /etc/nginx/sites-enabled/default`

`$ sudo vim /etc/nginx/sites-available/your_app`

Paste in the below. DNS name is either your elastic IP or the domain that points to your elastic IP.

```
server {
    listen       80;
    server_name  your_dnsname_here;

    location / {
        proxy_pass http://127.0.0.1:8000;
    }
}
```

If your backend application is communicating with a frontend application, you need to enable CORS headers for the domain of your frontend

```
server {
       listen 80;
       server_name your_dnsname_here;

       location / {

           set $cors '';
           if ($http_origin ~ '^https?://(localhost|www\.<domain>\.com|<domain>\.com)') {
                   set $cors 'true';
           }

          if ($cors = 'true') {
                  add_header 'Access-Control-Allow-Origin' "$http_origin" always;
                  # add_header 'Access-Control-Allow-Credentials' 'true';
                  add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
                  add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Mo    fied-Since,Keep-Alive,Origin,User-Agent,X-Requested-With' always;
                  # required to be able to read Authorization header in frontend
                  #add_header 'Access-Control-Expose-Headers' 'Authorization' always;
          }

          proxy_pass http://127.0.0.1:8000;
      }
  }
```

`$ sudo ln -s /etc/nginx/sites-available/your_app/etc/nginx/sites-enabled/`

`$ sudo nginx -t`

`$ sudo systemctl restart nginx`

## Run application

Install gunicorn and run application (Make sure wsgi.py file is serving your production database)

`$ pip install guincorn`

`$ gunicorn your_app.wsgi:application - bind 127.0.0.1`

If you visit your Elastic IP/domain you should now see your application running!

However this requires you to be connected to the server to run it. To solve, this we use `tmux`. Tmux is a package which turns your command line into a terminal. However, the feature we will be using today is the ability of it to run sessions. It allows you leave a session running that you can detach from, but the session will still continue executing its tasks. This allows you to easily spin up servers/execute long running tasks (eg. running a ML model). `Cntrl-C` to stop the application that is running and open up `tmux`

`$ tmux`

`$ gunicorn your_app.wsgi:application - bind 127.0.0.1`

`<Cntrl-B d>`

And you are done! The application is running the same way as before, but you can log out of your server and the application will still be running.

If you ever want to attach back to the tmux session:

`tmux attach -t <session>`

Check out more commands for tmux [here](http://www.dayid.org/comp/tm.html)

## Updating

If your application needs to be updated, attach to your tmux session, stop the application, pull the changes, restart the application. Not perfect, but it works!
