# ViajesGVR Deploy

Pasos mínimos para levantar la aplicación ViajesGVR usando Docker Compose.

---

## Requisitos previos

Antes de levantar el despliegue, se debe tener:

- Docker Desktop abierto y funcionando.
- Keycloak corriendo en:

```text
http://localhost:8080
```

- Realm de Keycloak configurado:

```text
viajesgvr
```

---

## 1. Levantar Keycloak

La aplicación usa Keycloak para el inicio de sesión y la autorización por roles.

Si Keycloak ya existe como contenedor Docker, se puede levantar con:

```bash
docker start <nombre-del-contenedor-keycloak>
```

Para verificar que Keycloak quedó corriendo:

```bash
docker ps
```

Luego revisar en el navegador:

```text
http://localhost:8080
```

---

## 2. Ir a la carpeta del despliegue

Abrir una terminal en la carpeta del repositorio:

```bash
cd viajesgvr-deploy
```

---

## 3. Levantar la aplicación

Ejecutar:

```bash
docker compose up
```

Esto levanta los servicios definidos en `compose.yml`:

- MySQL
- 3 réplicas del backend
- Balanceador Nginx para backend
- 3 réplicas del frontend
- Balanceador Nginx para frontend

---

## 4. Verificar contenedores

En otra terminal, ejecutar:

```bash
docker ps
```

Deberían aparecer contenedores asociados a:

- MySQL
- Backend
- Frontend
- Nginx
- Keycloak

---

## 5. Acceder a la aplicación

Frontend:

```text
http://localhost:8070
```

Backend:

```text
http://localhost:8090/api/tour-packages/
```

Si el backend responde con una lista o con:

```json
[]
```

significa que está funcionando.

---

## 6. Detener la aplicación

Para detener los servicios del despliegue:

```bash
docker compose down
```

Importante: no usar el siguiente comando si se quieren conservar los datos de la base de datos:

```bash
docker compose down -v
```

Ese comando elimina también el volumen de MySQL.

---

## 7. Prueba rápida de balanceo

Para detener una réplica del backend:

```bash
docker stop viajesgvr-backend-1
```

Luego probar nuevamente:

```text
http://localhost:8090/api/tour-packages/
```

La aplicación debería seguir funcionando.

Para volver a levantarla:

```bash
docker start viajesgvr-backend-1
```

Para detener una réplica del frontend:

```bash
docker stop viajesgvr-frontend-1
```

Luego probar nuevamente:

```text
http://localhost:8070
```

La aplicación debería seguir funcionando.

Para volver a levantarla:

```bash
docker start viajesgvr-frontend-1
```

---

## Resumen

Orden recomendado para iniciar todo:

```bash
docker start <nombre-del-contenedor-keycloak>
docker compose up
```

Luego abrir:

```text
http://localhost:8070
```
