# Advanced operations 

## Restarting containers 
There are atleast 4 main containers which are running as part of this installation through same docker-compose. The same docker-compose can be leveraged to restart any or all of these containers. 

Use [restart.sh](./bin/restart.sh) or Use below command to restart all containers
``` docker compose restart ```

To restart individual containers with name ( names:  nocodb, nginx, postgres, redis)\
ex: to restart nginx\
``` docker compose restart nginx ```

## Reload nginx 
use utility script at [./bin/nginx_reload.sh](./bin/nginx_reload.sh)

## [TBD] Upgrade nocodb instance 

## Enable SSL
To enable SSL for incoming https requests, nginx should be configured with combination of a public certificate and a private key. The SSL private key is kept secret on the server. It will be used to encrypt content sent to clients. 
Below are different approaches to get and configure certificates. Make your choice
### letsencrypt for generating certificates 
Certificates/key can be obtained by trusted CA (Certificate Authorities), there are many paid vendors found online or you can also use [letsencrypt](https://letsencrypt.org/) a non profit certificate provider for free however we recommend [donating](https://www.abetterinternet.org/donate/) for their work.

Run the script to create certificate using the script 
```
./bin/gen_letsencrypt_cert.sh 
```

### [TBD] Bring your own certificates 
If you already have the certificates, either self signed or generated by any other means, you will need to configure them with nginx. Below are the steps

### [TBD] Self signed certificates 
One of the pre-requisite is that your server should be associated with the domain name. In the absence of that you could use self signed certificates which does ecrypt but browsers show warning.

## Database password rotation 
As a security measure, It is best practice to rotate the database credentials periodically. Assuming you would have created new credentials in postgres database. The db credentials are persisted on filesystem as part of initial install and it will be available at 
[./conf/nc_properties.env](./conf/nc_properties.env)\
update properties POSTGRES_USER, POSTGRES_PASSWORD with new credentials and [restarting nocodb](#restarting-containers) with\
```docker compose restart nocodb```

## nginx configurations 
There are two main directories where nginx configurations are maintained 
- nocodb team managed configurations at [nginx/conf.d](./conf/nginx/conf.d).
- self managed (you) [conf/nginx/conf.d](./conf/nginx/conf.d) 

## postgres configurations 
[postgres.conf](./data/postgres/postgresql.conf) and [pg_hba.conf](./data/postgres/pg_hba.conf) are created under ./data/postgres directory upon first postgres container creation. The configurations can be updated and restarted continer to take affect.