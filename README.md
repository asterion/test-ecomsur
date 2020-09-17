# Instalación

## Clonar repositorio

```
git clone git@github.com:asterion/test-ecomsur.git
```
## Dominio local

Crear un dominio del tipo prueba.ecomsur.marcosmatamala apuntando
a 127.0.0.1 en su /etc/hosts o equivalente según su sistema operativo.

## Docker

Descargar el docker composer file [docker-compose.yml](https://gist.githubusercontent.com/asterion/cba1f5508a93503c0033fe994ddf50ff/raw/ab0d375dfcc75ca12343a6710ced9f6f83660545/docker-compose.yml)

Modificar WEB_ALIAS_DOMAIN por el dominio local definido anteriormente.

Modificar el string mmatamala a gusto.

Cambiar el string #CODIGO_PATH# por el path del directorio donde se clono el repositorio.

### Docker up

Los servicios utilizan los puertos 80, 443, 32823, 13306 y 8080 por lo tanto deben estar disponibles.

En el directorio donde se encuentre el docker-compose.yml levantar los contenedores/servicios docker.

```
docker-compose up -d --build
``` 

## Instalar Magento

### Conectarse al container del servicio web 

El nombre *web-mmatamala* del servicio web debe corresponder al definido en 
*docker-compose.yml*.

```
docker exec -it web-mmatamala bash
```

Se debe ingresar al directorio /app una vez conectado al servio web.

Descargar el script [instalador](https://gist.githubusercontent.com/asterion/05d91d9016274450c3395be49468100f/raw/cace12cd9ed3d211bcfba4628b5b2cbd6c189657/magento.sh) de magento.

Modificar script de instalación segun sus datos, para los datos del administrador, el host(mysql-mmatamala) del servicio de base de datos definido en docker-compose.yml y modificar la URL con el dominio local.

# Extras

En caso de problemas, utilice alguno de estos(o todos) comandos.

```
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
docker rmi $(docker images -q)
```

```
php bin/magento module:disable {Magento_TwoFactorAuth,Magento_Elasticsearch,Magento_InventoryElasticsearch,Magento_Elasticsearch6,Magento_Elasticsearch7}
php bin/magento setup:di:compile
php bin/magento setup:uninstall
php bin/magento cache:clean
php bin/magento cache:flush
php bin/magento setup:static-content:deploy -f
php bin/magento deploy:mode:set developer
php bin/magento dev:source-theme:deploy
php bin/magento indexer:reindex
php bin/magento dev:template-hints:enable
```
