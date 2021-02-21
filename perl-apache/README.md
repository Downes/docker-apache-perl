downes/apache-perl
==========

This is a base Apache-Perl container, used to initialize containers
with specific applications, such as gRSShopper.

gRSShopper is a tool that aggregates, organizes and distributes resources to support online learning. Read more here: https://grsshopper.downes.ca/


Docker image is here: https://hub.docker.com/r/downes/grsshopper-ple

**Note: don't use Docker image just now, run from this GitHub repository**

To run from Docker Image:
=========================
 
```
docker pull downes/apache-perl

docker run -p 80:80 -p 443:443 --detach --name app downes/apache-perl
```


OR, run from the GitHub repository:
==================================

Process:

 
```
git clone  https://github.com/Downes/docker-apache-perl
```

        (or git pull origin master if reloading the changed repo)


Run from Compose
----------------

```
cd docker-apache-perl

docker-compose up --build
```

OR, Build and Run Docker image
------------------------------

```
cd perl-apache

docker build --tag perl-apache .

docker run -p 443:443 --detach --name app downes/perl_apache
```


Testing the server 
==================

http://[your domain]  (should show Apache start page)

http://[your domain]/cgi-bin/apache_test.cgi  (should show Perl test page)     



Restart the Apache server
=========================

If Perl CGI isn't running properly, try:
```
docker exec -it gr1 /etc/init.d/apache2 reload
```

   (you can't docker exec -it bb3 apache2ctl restart because it crashes the entire container - see https://stackoverflow.com/questions/37523338/how-to-restart-apache2-without-terminating-docker-container )

   ( if you crash it, docker start bb3 )


Open a terminal in the container
================================
```
docker exec -e TERM=xterm -i -t gr1 bash
```

(A lot of sites say ```docker exec -it gr1 bash``` but I find it generates an error. Also if you want to be able to use nano when entering a container, you will need to set -e TERM=xterm).

