## Keystore

To create a keystore, enter the following:
```
sudo keytool -genkey \
    -alias georchestra_localhost \
    -keystore /srv/jetty/jetty80/etc/keystore \
    -storepass yourpassword \
    -keypass yourpassword \
    -keyalg RSA \
    -keysize 2048 \
    -dname "CN=geo.viennagglo.fr, OU=geo.viennagglo.fr, O=Unknown, L=Unknown, ST=Unknown, C=FR"
```
... where ```STOREPASSWORD``` is a password you choose, and the ```dname``` string is customized.

### CA certificates

If the geOrchestra webapps have to communicate with remote HTTPS services, our keystore/trustore has to include CA certificates.

This will be the case when:
 * the proxy has to relay an https service
 * geonetwork will harvest remote https nodes
 * geoserver will proxy remote https ogc services

To do this:
```
sudo keytool -importkeystore \
    -srckeystore /etc/ssl/certs/java/cacerts \
    -destkeystore /srv/jetty/jetty80/etc/keystore
```
First password is "yourpassword"     
The password of the srckeystore is "changeit" by default, and should be modified in /etc/default/cacerts.

### SSL

As the SSL certificate is absolutely required, at least for the CAS module, you must add it to the keystore.
```
sudo keytool -import -alias cert_ssl \
	-file /var/www/georchestra/ssl/georchestra.crt \
	-keystore /srv/jetty/jetty80/etc/keystore
```
Firts password is "yourpassword"     
After, answer yes or oui
