# MongoDB en Docker Compose Template

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat&logo=mongodb&logoColor=white)

---

## Requisitos Previos

Antes de usar esta plantilla, asegúrese de tener instalado:

- **Docker Desktop** (Windows/Mac) o **Docker Engine** (Linux) — versión 20.x o superior
- **Docker Compose** — incluido en Docker Desktop, o instalarlo por separado en Linux
- **Git** — para clonar y gestionar la plantilla

---

## Variables de Entorno y Seguridad

Nunca se deben incluir credenciales directamente en el archivo `docker-compose.yml`. La práctica estándar es utilizar un archivo `.env` que es ignorado por Git.

### `.env.example`

```ini
# ─── MongoDB ────────────────────────────────
MONGO_ROOT_USER=admin
MONGO_ROOT_PASSWORD=changeme_use_strong_password
MONGO_DATABASE=myapp_db
MONGO_PORT=27017

# ─── Mongo Express ──────────────────────────
ME_USERNAME=admin
ME_PASSWORD=changeme_express_password
ME_PORT=8081
```

### Pasos para configurar las variables

```bash
# 1. Copiar el archivo de ejemplo
cp .env.example .env

# 2. Editar con credenciales reales y seguras
nano .env   # o code .env en VS Code

# 3. Verificar que .env está en .gitignore
grep ".env" .gitignore
```

> **Advertencia de seguridad:** En entornos de producción, considere usar herramientas como **Docker Secrets**, **HashiCorp Vault** o servicios de secrets managers de la nube (AWS Secrets Manager, Azure Key Vault) en lugar de archivos `.env`.

## Comandos Útiles

Una vez configurada la plantilla, estos son los comandos más utilizados:

### Gestión de servicios

```bash
# Iniciar todos los servicios en segundo plano
docker compose up -d

# Ver el estado de los contenedores
docker compose ps

# Ver logs en tiempo real
docker compose logs -f

# Ver logs solo de MongoDB
docker compose logs -f mongodb

# Detener los servicios (sin eliminar datos)
docker compose down

# Detener y eliminar volúmenes (PELIGRO: borra todos los datos)
docker compose down -v
```

### Gestión de MongoDB

```bash
# Conectarse a la shell de MongoDB
docker exec -it mongodb mongosh \
  -u $MONGO_ROOT_USER \
  -p $MONGO_ROOT_PASSWORD \
  --authenticationDatabase admin

# Crear un backup
docker exec mongodb mongodump \
  --uri="mongodb://$MONGO_ROOT_USER:$MONGO_ROOT_PASSWORD@localhost:27017" \
  --out=/data/backup

# Restaurar desde backup
docker exec mongodb mongorestore \
  --uri="mongodb://$MONGO_ROOT_USER:$MONGO_ROOT_PASSWORD@localhost:27017" \
  /data/backup
```

### MongoDB Express UI

El proyecto incluye un contenedor para mongo express con el profile UI, pero prefiera utilizar esto en maquina dado que la imagen
de docker se mantiene desactualizada.

Más detalles en [Mongo-Express GH repo](https://github.com/mongo-express/mongo-express).

## Estructura del Proyecto

```
📦 mongodb-docker-template
 ┣ 📂 mongo
 ┃ └ 📜 mongod.conf
 ┣ 📂 backups
 ┃ └ 📜 .gitkeep
 ┣ 📜 .env.example
 ┣ 📜 .gitignore
 ┣ 📜 docker-compose.yml
 └ 📜 README.md
```
