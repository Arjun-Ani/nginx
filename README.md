# nginx

This will have the nginxconfiguration which will redirect the server according to client location.For that we have to install nginx with geoip support(If we are compiling ngingneed to add geoip module)

#nginx -V {This will list all the modules of nginx and for redirection according to client location we need geoip module so make sure that it is available or not}

Need to download the geoip database
#wget -N http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz
#gunzip GeoLiteCity.dat.gz
#mv GeoLiteCity.dat /usr/share/GeoIP/

Now we need to add the loation of geoip database in nginx.conf

geoip_country /usr/share/GeoIP/GeoIP.dat;
geoip_city /usr/share/GeoIP/GeoLiteCity.dat;

Now the variable geoip_city_country_code will give the country code of the viewer and we can use that variable in conf to check the viewer's location and then redirect accordingly.

Code for redirection :-

if ($geoip_city_country_code = "IN"){
		rewrite ^ $scheme://www.google.in$request_uri break;
	}
	if ($geoip_city_country_code = "HK"){
                rewrite ^ $scheme://www.google.hk$request_uri break;
        }

We can also debug the variable by writing it to debug header's

add_header X-uri "$geoip_city_country_code";
