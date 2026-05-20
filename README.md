# 🐳 Donatón - Docker Compose

Orquestación completa del proyecto Donatón con Docker Compose.

## 📋 Servicios Incluidos

| Servicio | Puerto | Descripción |
|----------|--------|-------------|
| **MySQL** | 3306 | Base de datos central |
| **Donaciones Service** | 8082 | API Microservicio Donaciones |
| **Necesidades Service** | 8083 | API Microservicio Necesidades |
| **Logística Service** | 8084 | API Microservicio Logística |
| **API Gateway** | 8080 | Gateway unificado (BFF) |
| **Frontend React** | 5173 | Aplicación web Donatón |

## 🚀 Cómo Ejecutar

### Opción 1: Con Docker Compose (Recomendado)

```bash
# Clonar o descargar el proyecto
cd donaton-dockercompose

# Crear y ejecutar todos los contenedores
docker-compose up --build

# Para ejecutar en segundo plano
docker-compose up -d --build
```

**Acceso a servicios**:
- 🌐 Frontend: http://localhost:5173
- 🔌 API Gateway: http://localhost:8080
- 📦 Donaciones API: http://localhost:8082/swagger-ui.html
- 📋 Necesidades API: http://localhost:8083/swagger-ui.html
- 🚚 Logística API: http://localhost:8084/swagger-ui.html
- 🗄️ MySQL: localhost:3306 (usuario: root, password: root123)

### Opción 2: Ejecutar Servicios Específicos

```bash
# Solo base de datos
docker-compose up -d mysql

# Solo backend sin frontend
docker-compose up -d mysql donaciones-service necesidades-service logistica-service api-gateway

# Solo frontend
docker-compose up frontend
```

## 📊 Estructura de Bases de Datos

**3 esquemas independientes en MySQL**:

```
donaton_donaciones
├── donaciones (12 registros de seed)

donaton_necesidades
├── necesidades (12 registros de seed)

donaton_logistica
├── centros_acopio (12 registros de seed)
└── envios (12 registros de seed con FK)
```

**Total: 48 registros de seed data cargados automáticamente**

## 🛑 Detener Contenedores

```bash
# Detener todos los servicios
docker-compose down

# Detener y eliminar volúmenes (CUIDADO: borra datos)
docker-compose down -v
```

## 📝 Logs de Servicios

```bash
# Ver logs de todos los servicios
docker-compose logs -f

# Ver logs de un servicio específico
docker-compose logs -f frontend
docker-compose logs -f api-gateway
docker-compose logs -f donaciones-service
```

## 🔍 Ver Estado de Contenedores

```bash
docker-compose ps
```

## 🔗 Red de Servicios

Todos los servicios están conectados en la red `donaton-network`, permitiendo comunicación interna:

- Los microservicios se comunican entre sí usando nombres de contenedor
- Ejemplo: `http://donaciones-service:8082/api/v1/donaciones`
- El frontend accede al API Gateway internamente

## 📦 Volúmenes

- `mysql_data`: Almacena datos persistentes de MySQL

## ⚙️ Variables de Entorno

### MySQL
```
MYSQL_ROOT_PASSWORD: root123
```

### Microservicios
```
SPRING_DATASOURCE_USERNAME: root
SPRING_DATASOURCE_PASSWORD: root123
SPRING_JPA_HIBERNATE_DDL_AUTO: validate
```

### Frontend
```
VITE_BFF_URL: http://api-gateway:8080/api
```

## 🧪 Testing del Proyecto

### 1. Verificar que todo está corriendo
```bash
docker-compose ps
```

### 2. Acceder a la API
```bash
# Listar donaciones
curl http://localhost:8080/api/v1/donaciones

# Crear nueva donación
curl -X POST http://localhost:8080/api/v1/donaciones \
  -H "Content-Type: application/json" \
  -d '{
    "tipoRecurso": "ALIMENTO",
    "detalleRecurso": "Arroz",
    "cantidad": 50,
    "origen": "Donante"
  }'
```

### 3. Acceder al Frontend
Abrir en navegador: http://localhost:5173

## 🔐 Consideraciones de Seguridad

⚠️ **IMPORTANTE**: Esta configuración es para **DESARROLLO LOCAL** únicamente.

Para **PRODUCCIÓN**, considere:

- ✅ Cambiar contraseña de MySQL
- ✅ Usar variables de entorno desde archivo `.env`
- ✅ Implementar autenticación (JWT/OAuth2)
- ✅ Usar secrets en lugar de variables planas
- ✅ Configurar CORS restrictivo
- ✅ Usar HTTPS/TLS
- ✅ Implementar circuit breaker
- ✅ Agregar rate limiting

## 🆘 Solución de Problemas

### Puerto en uso
```bash
# Cambiar puerto en docker-compose.yml
# Ejemplo: cambiar 5173:5173 a 5174:5173
```

### Base de datos no conecta
```bash
# Verificar que mysql está corriendo
docker-compose logs mysql

# Reiniciar mysql
docker-compose restart mysql
```

### Frontend no carga
```bash
# Verificar logs del frontend
docker-compose logs frontend

# Asegurar que API Gateway está corriendo
docker-compose logs api-gateway
```

## 📚 Recursos Adicionales

- **API Documentation**: Ver swagger en cada servicio
- **Backend Docs**: Revisar README en cada carpeta de servicio
- **Frontend Docs**: Revisar código en `donaton-front/src`

## 👥 Autores

Proyecto Donatón - Evaluación 2

---

**Última actualización**: Enero 2025
**Versión**: 2.0 (Completa con seed data y frontend dockerizado)
