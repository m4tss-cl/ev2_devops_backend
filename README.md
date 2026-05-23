# EV2 DevOps Backend

Repositorio backend compuesto por 2 microservicios Spring Boot:

- **Springboot-API-REST-ventas**: gestión de ventas.
- **Springboot-API-REST-DESPACHO**: gestión de despachos.

Además, el repositorio incluye un `compose-back.yml` para levantar ambos servicios con sus bases de datos MySQL.

## Arquitectura del proyecto

```text
ev2_devops_backend/
├── Springboot-API-REST-ventas/
├── Springboot-API-REST-DESPACHO/
├── compose-back.yml
└── README.md
```

## Tecnologías

- Java 17
- Spring Boot 3.4.x
- Spring Data JPA
- MySQL 8.4
- Swagger / OpenAPI (springdoc)
- Docker + Docker Compose
- Maven Wrapper (`mvnw`)

## Requisitos

- Docker y Docker Compose
- (Opcional para ejecución local sin Docker) Java 17

## Levantar el entorno completo con Docker Compose

Desde la raíz del repositorio:

```bash
docker compose -f compose-back.yml up -d
```

Servicios y puertos expuestos (formato `host:contenedor`):

- **Despacho API**: `8080:8081` → acceso en `http://localhost:8080`
- **Ventas API**: `8081:8080` → acceso en `http://localhost:8081`
- **MySQL despacho**: `localhost:3306`
- **MySQL ventas**: `localhost:3307`

Para detener:

```bash
docker compose -f compose-back.yml down
```

## Variables de entorno principales

Cada microservicio espera las siguientes variables para conexión a base de datos:

- `DB_ENDPOINT`
- `DB_PORT`
- `DB_NAME`
- `DB_USERNAME`
- `DB_PASSWORD`

En `compose-back.yml` se inyectan mediante `SPRING_DATASOURCE_*` para cada contenedor.

## Ejecución local de cada microservicio (sin Docker)

### Ventas

```bash
cd Springboot-API-REST-ventas
bash ./mvnw spring-boot:run
```

### Despacho

```bash
cd Springboot-API-REST-DESPACHO
bash ./mvnw spring-boot:run
```

> Nota: para ejecución local debes definir las variables de entorno de base de datos indicadas arriba.

## Endpoints principales

### Ventas (`/api/v1/ventas`)

- `POST /api/v1/ventas`
- `GET /api/v1/ventas`
- `GET /api/v1/ventas/{idVenta}`
- `PUT /api/v1/ventas/{idVenta}`
- `DELETE /api/v1/ventas/{idVenta}`

### Despachos (`/api/v1/despachos`)

- `POST /api/v1/despachos`
- `GET /api/v1/despachos`
- `GET /api/v1/despachos/{idDespacho}`
- `PUT /api/v1/despachos/{idDespacho}`
- `DELETE /api/v1/despachos/{idDespacho}`

## Swagger / OpenAPI

Con los servicios en ejecución:

- Ventas: `http://localhost:8081/swagger-ui.html`
- Despacho: `http://localhost:8080/swagger-ui.html`

## Pruebas

Para ejecutar pruebas unitarias/integración por servicio:

```bash
cd Springboot-API-REST-ventas
bash ./mvnw test

cd ../Springboot-API-REST-DESPACHO
bash ./mvnw test
```

Si no se definen variables de entorno de base de datos, los tests de contexto Spring pueden fallar al inicializar la conexión MySQL.
