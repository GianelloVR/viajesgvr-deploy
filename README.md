# ViajesGVR Deploy

Repositorio de despliegue Docker para la aplicación ViajesGVR.

Este repositorio contiene la configuración necesaria para levantar el sistema usando Docker Compose, tanto en ambiente local como en AWS.

---

## Servicios incluidos

El despliegue levanta los siguientes servicios:

- MySQL
- Keycloak
- Backend Spring Boot
- Frontend React
- Balanceador Nginx para backend
- Balanceador Nginx para frontend

En AWS se trabaja con varias instancias del backend y frontend para simular balanceo de carga.

---

## Requisitos previos

Antes de levantar el proyecto se necesita tener instalado:

- Docker
- Docker Compose
- Git

Además, las imágenes deben estar disponibles en Docker Hub:

```bash
gianellovr/viajesgvr-backend:latest
gianellovr/viajesgvr-frontend:latest
```

---

## Levantar en ambiente local

Desde la carpeta del repositorio:

```bash
docker compose up -d
```

Verificar que los contenedores estén funcionando:

```bash
docker ps
```

Accesos locales:

```text
Frontend:  http://localhost:8070
Backend:   http://localhost:8090/api/tour-packages/
Keycloak:  http://localhost:8080
```

---

## Detener ambiente local

```bash
docker compose down
```

No usar `-v` si se quieren conservar los datos de MySQL y Keycloak.

---

## Levantar en AWS

Conectarse por SSH a la instancia EC2 y entrar a la carpeta del despliegue:

```bash
cd ~/viajesgvr-deploy
```

Descargar las últimas imágenes:

```bash
docker pull gianellovr/viajesgvr-backend:latest
docker pull gianellovr/viajesgvr-frontend:latest
```

Levantar los servicios:

```bash
docker compose -f compose.aws.yml up -d
```

Verificar contenedores:

```bash
docker ps
```

Acceso en AWS:

```text
https://<IP_PUBLICA_EC2>:8070
```

En el despliegue usado para la entrega:

```text
https://18.216.240.192:8070
```

---

## Detener ambiente AWS

Desde la carpeta del despliegue en la instancia EC2:

```bash
docker compose -f compose.aws.yml down
```

No usar `-v` si se quieren conservar los datos de MySQL y Keycloak.

---

## Usuarios de prueba

Administrador:

```text
Usuario: admin
Contraseña: admin123
```

Cliente:

```text
Usuario: cliente
Contraseña: cliente123
```

---

## Prueba rápida del sistema

Una vez levantado el sistema:

1. Entrar al frontend.
2. Iniciar sesión como administrador.
3. Ir a Panel Admin.
4. Crear un paquete turístico.
5. Verificar que el paquete aparezca en la lista.
6. Cerrar sesión.
7. Iniciar sesión como cliente.
8. Verificar que el cliente pueda ver los paquetes.

---

## Comandos útiles

Ver contenedores activos:

```bash
docker ps
```

Ver logs del backend:

```bash
docker logs viajesgvr-backend-1 --tail=80
```

Ver logs del balanceador backend:

```bash
docker logs viajesgvr-backend-balancer --tail=80
```

Ver logs del balanceador frontend:

```bash
docker logs viajesgvr-frontend-balancer --tail=80
```

Ver logs de Keycloak:

```bash
docker logs viajesgvr-keycloak --tail=80
```

Ver logs de MySQL:

```bash
docker logs viajesgvr-mysql --tail=80
```

---

## Archivos principales

```text
compose.yml                  Configuración para ambiente local
compose.aws.yml              Configuración para ambiente AWS
nginx-backend.conf           Balanceador del backend
nginx-frontend.conf          Balanceador del frontend local
nginx-frontend-aws.conf      Configuración frontend AWS sin SSL
nginx-frontend-aws-ssl.conf  Configuración frontend AWS con HTTPS
viajesgvr-realm.json         Configuración inicial de Keycloak
```

---

## Notas importantes

La versión local se levanta con:

```bash
docker compose up -d
```

La versión AWS se levanta con:

```bash
docker compose -f compose.aws.yml up -d
```

La carpeta `certs/` no se sube al repositorio porque puede contener certificados o claves privadas.

El archivo `README_detalle.md` no forma parte del despliegue final y no se sube al repositorio.

La configuración CORS del backend permite el origen local y el origen AWS usado para la entrega.
