FROM ubuntu:20.10

LABEL Description="Perl and Apache with SSL, based on Ubuntu 20.10 LTS." \
	License="Apache License 2.0" \
	Usage="docker run -d -p [HOST WWW PORT NUMBER]:80 -p [HOST DB PORT NUMBER]:3306 -v [HOST WWW DOCUMENT ROOT]:/var/www/html -v downes/apache-perl" \
	Version="1.0" \
	Maintainer="Stephen Downes <stephen@downes.ca>"

RUN apt-get update
RUN apt-get upgrade -y

COPY debconf.selections /tmp/
RUN debconf-set-selections /tmp/debconf.selections

RUN apt-get install -y zip unzip
	
RUN apt-get install -y cpanminus 
RUN apt-get install -y	  liblocal-lib-perl 
RUN apt-get install -y      libcgi-session-perl 
RUN apt-get install -y 	    libdbd-mysql-perl
RUN apt-get install -y      libwww-perl 
RUN apt-get install -y      libmime-types-perl 
RUN apt-get install -y      libjson-perl 
RUN apt-get install -y      libjson-xs-perl
RUN apt-get install -y      libjson-parse-perl 
RUN apt-get install -y      libtypes-datetime-perl 
RUN apt-get install -y     libcrypt-eksblowfish-perl 
RUN apt-get install -y      libtext-vcard-perl 
RUN apt-get install -y      libfile-slurp-perl 
RUN apt-get install -y      liblingua-en-inflect-number-perl 
RUN apt-get install -y	  libemail-stuffer-perl 
RUN apt-get install -y	  libimage-magick-perl 
RUN apt-get install -y	  librest-application-perl 
RUN apt-get install -y 	  libplack-perl
RUN apt-get install -y	  libauthen-simple-perl  
RUN apt-get install -y	  libauthen-simple-net-perl 
RUN apt-get install -y	  libcgi-xml-perl 
RUN apt-get install -y	  libxml-opml-perl
RUN apt-get install -y    libtemplate-plugin-gd-perl 
RUN apt-get install -y    libwww-curl-perl
RUN apt-get install -y    libwww-mechanize-perl

RUN cpanm Image::Resize
RUN cpanm Mastodon::Client
RUN cpanm Net::Twitter::Lite::WithAPIv1_1
RUN cpanm REST::Client
RUN cpanm MIME::Lite::TT::HTML

      
RUN apt-get install apache2 -y
RUN apt-get install git -y
RUN apt-get install nano -y
RUN apt-get install curl -y
RUN apt-get install ftp -y


ENV LOG_STDOUT **Boolean**
ENV LOG_STDERR **Boolean**
ENV LOG_LEVEL warn
ENV ALLOW_OVERRIDE All
ENV DATE_TIMEZONE UTC
ENV TERM dumb

COPY cgi-enabled.conf /etc/apache2/conf-available/
COPY ssl-params.conf /etc/apache2/conf-available/
COPY default-ssl.conf /etc/apache2/conf-available/

RUN a2enmod rewrite
RUN a2enmod ssl
RUN a2enmod headers 
RUN a2enmod cgid 
RUN rm -f /etc/apache2/conf-available/serve-cgi-bin.conf 
RUN a2ensite default-ssl
RUN a2enconf cgi-enabled 

# Set up server test
RUN mkdir /var/www/html/cgi-bin
COPY apache_test.cgi /var/www/html/cgi-bin
RUN chmod 705 /var/www/html/cgi-bin/apache_test.cgi

# Run startup script
COPY run-lamp.sh /usr/sbin/
RUN chmod +x /usr/sbin/run-lamp.sh
RUN chown -R www-data:www-data /var/www/html

VOLUME /var/www/html
VOLUME /var/log/httpd
VOLUME /var/lib/mysql
VOLUME /var/log/mysql
VOLUME /etc/apache2

EXPOSE 80
EXPOSE 443
EXPOSE 3306

CMD ["/usr/sbin/run-lamp.sh"]
