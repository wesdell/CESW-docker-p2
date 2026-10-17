# MI APRENDIZAJE
Antes de realizar la práctica solo sabía usar imágenes que ya existían en Docker Hub. Con esta práctica aprendí lo siguiente:

- Se puede limitar la memoria y el procesador que usa un contenedor, para que no afecte a otros contenedores o a la computadora.
- La memoria swap que puede usar un contenedor se obtiene restando el valor de --memory al valor de --memory-swap.
- Con un healthcheck Docker puede revisar cada cierto tiempo si el contenedor está funcionando bien.
- Puedo crear mis propias imágenes con un Dockerfile y construirlas con docker build.
- Docker guarda en caché los pasos que no cambian, por eso al crear la segunda versión de la imagen solo se volvió a ejecutar el paso COPY y fue mucho más rápido.
- Con la opción -P Docker asigna automáticamente un puerto del host a los puertos del contenedor.
- Las imágenes huérfanas son imágenes sin nombre que ocupan espacio y se pueden eliminar con docker image prune.
- Las políticas de reinicio permiten decidir si un contenedor se vuelve a iniciar cuando se detiene.

### Problema encontrado
Al construir la imagen con el Dockerfile salía el error `Cannot find a valid baseurl for repo: base/7/x86_64` en el paso `RUN yum -y update`. Esto pasa porque centos:7 ya no tiene soporte y sus repositorios fueron movidos a vault.centos.org. Para solucionarlo se agregó la siguiente línea en el Dockerfile después del FROM:
```
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-* && sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
```

### Otros comandos utilizados
Ver el puerto del host asignado a un contenedor
```
docker port server-apache
```
