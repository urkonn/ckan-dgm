### CKAN PLUGINS
Dockerizacion de una instancia de CKAN + plugins para datos.gob.mx.

# Deployment Kubernetes
Para implementaciones basadas en kubernetes usar la siguiente plantilla.

```sh
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
 name: ckan
spec:
 replicas: 1
 template:
   metadata:
     labels:
       app: ckan
       tier: frontend
   spec:
     containers:
       - name: ckan
         image: mxabierto/ckan-dgm:v2.0.{version}
         ports:
           - containerPort: 5000
         env:
           - name: INIT_DBS
             value: "false"
           - name: CKAN_SITE_URL
             value: "https://datos.gob.mx/busca"
           - name: SOLR_PORT_8080_TCP_ADDR
             value: "{xxxxxxx}"
           - name: SOLR_PORT_8080_TCP_PORT
             value: "{puerto-solr}"
           - name: POSTGRES_PORT_5432_TCP_ADDR
             value: "{xxxxxx}"
           - name: POSTGRES_PORT_5432_TCP_PORT
             value: "{puerto-postgres}"
           - name: POSTGRES_ENV_POSTGRES_DB
             value: "{ckan-databse}"
           - name: POSTGRES_ENV_POSTGRES_USER
             value: "{ckan-user}"
           - name: POSTGRES_ENV_POSTGRES_PASSWORD
             value: "{xxxxxx}"
           - name: DATASTORE_ENV_USER_DATASTORE
             value: "{ckan-datastore-user-write}"
           - name: DATASTORE_ENV_USER_DATASTORE_PWD
             value: "{datastore-user-password}"
           - name: DATASTORE_ENV_USER_DATASTORE_READ
             value: "{datastore-user-read}"
           - name: DATASTORE_PORT_5432_TCP_ADDR
             value: "{datastore-server-ip}"
           - name: DATASTORE_ENV_DATABASE_DATASTORE
             value: "{datastore-dbname}"
           - name: RABBIT_HOSTNAME
             value: "{rabbit_tcp_ip}"
           - name: RABBIT_PASSWORD
             value: "{rabbit_password}"
           - name: RABBIT_PORT
             value: "{rabbit_port}"
           - name: RABBIT_USER
             value: "{rabbit_user}"
           - name: RABBIT_VHOST
             value: /
           - name: DATAPUSHER_URL_WITH_PORT
             value: http://{ip-ckan}:8800/
         volumeMounts:
           - mountPath: "/var/lib/ckan"
             name: ckan-data
     nodeSelector:
       name: worker-02
     volumes:
       - name: ckan-data
         hostPath:
           path: "xxxxxxxxxxxxxxxxx"
```

## Release Notes

- Cambio de nombre en variable de ambiente para SOLR (SOLR_PORT_8080_TCP_PORT, SOLR_PORT_8080_TCP_PORT)

- Dependencia con instancias REDIS para manejo de background-jobs (REDIS_PORT_6379_TCP_ADDR)

- Cambio de nombre en variables de ambiente para DATASTORE:
  - DATASTORE_ENV_DATABASE_DATASTORE
  - DATASTORE_ENV_USER_DATASTORE
  - DATASTORE_ENV_USER_DATASTORE_READ
  - DATASTORE_ENV_USER_DATASTORE_PWD
  - DATASTORE_PORT_5432_TCP_ADDR
  - DATAPUSHER_URL_WITH_PORT

- Cambio de nombre en variable de ambiente para SOLR (SOLR_PORT_8080_TCP_PORT, SOLR_PORT_8080_TCP_PORT)

- Dependencia con instancias RabbitMq para manejo de background-jobs (RABBIT_HOSTNAME)
