# Self-signed certificate

Generate a private key (enter a good passphrase and keep it safe !)
```
sudo rm -Rf /var/www/georchestra/ssl/*

sudo openssl genrsa -des3 \
	-passout pass:yourpassword \
	-out /var/www/georchestra/ssl/georchestra.key 2048
```

Protect it with:
```
sudo chmod 400 /var/www/georchestra/ssl/georchestra.key
```

Generate a [Certificate Signing Request](http://en.wikipedia.org/wiki/Certificate_signing_request) (CSR) for this key, with eg:
```
sudo openssl req \
	-key /var/www/georchestra/ssl/georchestra.key \
	-subj "/C=FR/ST=None/L=None/O=None/OU=None/CN=georchestra.fr" \
	-newkey rsa:2048 -sha256 \
	-passin pass:yourpassword \
	-out /var/www/georchestra/ssl/georchestra.csr
```

Be sure to replace the ```/C=FR/ST=None/L=None/O=None/OU=None/CN=geo.viennagglo.fr``` string with something more relevant:
 * ```C``` is the 2 letter Country Name code
 * ```ST``` is the State or Province Name
 * ```L``` is the Locality Name (eg, city)
 * ```O``` is the Organization Name (eg, company)
 * ```OU``` is the Organizational Unit (eg, company department)
 * ```CN``` is the Common Name (***your server FQDN***)

Create an unprotected key:
```
sudo openssl rsa \
	-in /var/www/georchestra/ssl/georchestra.key \
	-passin pass:yourpassword \
	-out /var/www/georchestra/ssl/georchestra-unprotected.key
```

Finally generate a self-signed certificate (CRT):
```
sudo openssl x509 -req \
	-days 365 \
	-in /var/www/georchestra/ssl/georchestra.csr \
	-signkey /var/www/georchestra/ssl/georchestra.key \
	-passin pass:yourpassword
	-out /var/www/georchestra/ssl/georchestra.crt
```

We check folder's content :
```
sudo chown -Rf www-data:www-data /var/www/georchestra/ssl/*
ls -l /var/www/georchestra/ssl/
```

Restart the web server:
```
sudo service apache2 restart
``` 
