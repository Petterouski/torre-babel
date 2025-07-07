📋 Descripción
Room Create Service es un microservicio desarrollado en Python que forma parte del sistema de gestión hotelera eco-amigable. Este servicio se encarga específicamente de la creación de habitaciones en el sistema, siguiendo los principios de arquitectura de microservicios y patrones de diseño MVC + DRY.
🏗️ Arquitectura

Patrón de Diseño: MVC (Model-View-Controller) + DRY (Don't Repeat Yourself)
Estilo de Arquitectura: RESTful API
Base de Datos: PostgreSQL
Lenguaje: Python 3.11+
Framework: FastAPI
Dominio de Negocio: Room Management Domain

🚀 Características

✅ Creación de habitaciones con validación completa
✅ Autenticación JWT con middleware personalizado
✅ Logging avanzado con rotación de archivos
✅ Validación de datos con reglas de negocio
✅ Manejo de errores robusto
✅ Documentación automática con Swagger
✅ Pruebas unitarias e integración
✅ Soporte CORS configurable
✅ Contenedorización con Docker
✅ CI/CD con GitHub Actions

📁 Estructura del Proyecto
room-create-service/
├── app/
│   ├── main.py                    # Aplicación principal FastAPI
│   ├── controllers/
│   │   └── room_controller.py     # Controlador de habitaciones
│   ├── services/
│   │   └── room_service.py        # Lógica de negocio
│   ├── repositories/
│   │   └── room_repository.py     # Acceso a datos
│   ├── models/
│   │   └── room_model.py          # Modelo de base de datos
│   ├── config/
│   │   ├── database.py            # Configuración de BD
│   │   └── logging_config.py      # Configuración de logs
│   ├── utils/
│   │   ├── validators.py          # Validadores de datos
│   │   ├── response_formatter.py  # Formateador de respuestas
│   │   └── business_rules.py      # Reglas de negocio
│   └── middleware/
│       ├── auth_middleware.py     # Middleware de autenticación
│       └── error_handler.py       # Manejador de errores
├── tests/
│   ├── test_room_creation.py      # Pruebas unitarias
│   └── integration/               # Pruebas de integración
├── logs/                          # Archivos de log
├── requirements.txt               # Dependencias Python
├── Dockerfile                     # Imagen Docker
└── README.md                      # Documentación
🛠️ Instalación
Prerrequisitos

Python 3.11+
PostgreSQL 15+
Docker (opcional)
Git

Instalación Local

Clonar el repositorio

bashgit clone https://github.com/tu-usuario/hotel-ecoapp-monorepo.git
cd hotel-ecoapp-monorepo/microservices/room-create-service

Crear entorno virtual

bashpython -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate

Instalar dependencias

bashpip install -r requirements.txt

Configurar variables de entorno

bashexport DATABASE_URL="postgresql://dpostgres2:dpostgres2@database-1.amazonaws.com:5432/rooms_db"
export JWT_SECRET_KEY="tu-clave-secreta-aqui"

Ejecutar la aplicación

bashuvicorn main:app --reload --host 0.0.0.0 --port 8000
Instalación con Docker

Construir la imagen

bashdocker build -t room-create-service .

Ejecutar el contenedor

bashdocker run -p 8000:8000 \
  -e DATABASE_URL="postgresql://dpostgres2:dpostgres2@database-1.amazonaws.com:5432/rooms_db" \
  -e JWT_SECRET_KEY="tu-clave-secreta-aqui" \
  room-create-service
🔧 Configuración
Variables de Entorno
VariableDescripciónValor por DefectoDATABASE_URLURL de conexión a PostgreSQLpostgresql://dpostgres2:dpostgres2@database-1.amazonaws.com:5432/rooms_dbJWT_SECRET_KEYClave secreta para JWTyour-secret-key-here
Base de Datos
El servicio se conecta a una base de datos PostgreSQL con las siguientes credenciales:

Host: database-1.amazonaws.com
Database: rooms_db
Usuario: dpostgres2
Contraseña: dpostgres2

Esquema de la Tabla rooms
sqlCREATE TABLE rooms (
    id SERIAL PRIMARY KEY,
    room_number VARCHAR(10) UNIQUE NOT NULL,
    room_type VARCHAR(50) NOT NULL,
    floor INTEGER NOT NULL,
    price_per_night DECIMAL(10,2) NOT NULL,
    capacity INTEGER NOT NULL,
    has_balcony BOOLEAN DEFAULT FALSE,
    has_ocean_view BOOLEAN DEFAULT FALSE,
    is_available BOOLEAN DEFAULT TRUE,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
🔌 API Endpoints
Autenticación
Todos los endpoints requieren autenticación JWT. Incluye el token en el header:
Authorization: Bearer <tu-jwt-token>
Endpoints Disponibles
1. Health Check
httpGET /health
Respuesta:
json{
  "status": "healthy",
  "service": "room-create-service"
}
2. Crear Habitación
httpPOST /api/v1/rooms
Content-Type: application/json
Authorization: Bearer <token>
Payload:
json{
  "room_number": "101",
  "room_type": "single",
  "floor": 1,
  "price_per_night": 75.0,
  "capacity": 1,
  "has_balcony": false,
  "has_ocean_view": false,
  "is_available": true,
  "is_active": true
}
Respuesta Exitosa (201):
json{
  "success": true,
  "message": "Room created successfully",
  "data": {
    "id": 1,
    "room_number": "101",
    "room_type": "single",
    "floor": 1,
    "price_per_night": 75.0,
    "capacity": 1,
    "has_balcony": false,
    "has_ocean_view": false,
    "is_available": true,
    "is_active": true,
    "created_at": "2024-01-01T10:00:00Z",
    "updated_at": "2024-01-01T10:00:00Z"
  }
}
Tipos de Habitación Válidos

single: Habitación individual
double: Habitación doble
suite: Suite
eco-suite: Suite ecológica

Reglas de Negocio

Eco-suites: Deben tener balcón obligatoriamente
Vista al mar: Precio mínimo de $100 por noche
Suites: Capacidad mínima de 2 huéspedes
Número de habitación: Único en el sistema

🧪 Pruebas
Ejecutar Pruebas Unitarias
bashpytest tests/ -v
Ejecutar Pruebas con Cobertura
bashpytest tests/ -v --cov=app --cov-report=html
Ejecutar Pruebas de Integración
bashpytest tests/integration/ -v
Análisis de Código
bash# Formateo con Black
black .

# Ordenar imports
isort .

# Linting con flake8
flake8 .

# Análisis estático con mypy
mypy . --ignore-missing-imports
📊 Logging
El servicio implementa logging estructurado con los siguientes niveles:

INFO: Operaciones normales
DEBUG: Información detallada para debugging
WARNING: Situaciones que requieren atención
ERROR: Errores que no detienen el servicio
CRITICAL: Errores críticos

Los logs se guardan en:

logs/room-create-service.log (rotación automática)
Salida estándar (stdout)

🔒 Seguridad
Autenticación JWT
El servicio utiliza JWT para autenticación. El token debe incluir:
json{
  "user_id": "identificador-del-usuario",
  "iat": "timestamp-de-creacion",
  "exp": "timestamp-de-expiracion"
}
CORS
CORS está configurado para aceptar todas las origen durante desarrollo. Para producción, configurar específicamente:
pythonapp.add_middleware(
    CORSMiddleware,
    allow_origins=["https://tu-dominio.com"],
    allow_credentials=True,
    allow_methods=["GET", "POST"],
    allow_headers=["*"],
)
🚀 Despliegue
CI/CD con GitHub Actions
El proyecto incluye un pipeline de CI/CD que se activa automáticamente:

Trigger: Push a la rama qa
Pruebas: Ejecuta todas las pruebas unitarias e integración
Análisis: Linting, formateo y análisis estático
Build: Construye la imagen Docker
Push: Sube la imagen a DockerHub
Seguridad: Escaneo de vulnerabilidades con Trivy

Secrets Requeridos en GitHub
bashDOCKERHUB_USERNAME=tu-usuario-dockerhub
DOCKERHUB_TOKEN=tu-token-dockerhub
Despliegue en AWS
El servicio está diseñado para desplegarse en AWS utilizando:

EC2: Para instancias de aplicación
RDS: Para base de datos PostgreSQL
Application Load Balancer: Para distribución de tráfico
Auto Scaling Group: Para escalabilidad automática

📚 Documentación API
Una vez que el servicio esté ejecutándose, puedes acceder a la documentación interactiva:

Swagger UI: http://localhost:8000/docs
ReDoc: http://localhost:8000/redoc

🤝 Contribución
Estándares de Código

Idioma: Todo el código debe estar en inglés
Formateo: Utilizar Black para formateo
Imports: Utilizar isort para organizar imports
Documentación: Todos los métodos deben tener docstrings
Commits: Seguir Conventional Commits

Proceso de Contribución

Fork del repositorio
Crear rama feature: git checkout -b feature/nueva-funcionalidad
Commit cambios: git commit -m 'feat: add nueva funcionalidad'
Push a la rama: git push origin feature/nueva-funcionalidad
Crear Pull Request

Conventional Commits
feat: add new feature
fix: bug fix
docs: update documentation
style: formatting changes
refactor: code refactoring
test: add tests
chore: maintenance tasks
🔧 Troubleshooting
Problemas Comunes
1. Error de Conexión a Base de Datos
sqlalchemy.exc.OperationalError: (psycopg2.OperationalError) could not connect to server
Solución: Verificar que PostgreSQL esté ejecutándose y las credenciales sean correctas.
2. Error de Autenticación JWT
401 Unauthorized: Invalid token
Solución: Verificar que el token JWT sea válido y no haya expirado.
3. Error de Validación
400 Bad Request: Room number already exists
Solución: Verificar que el número de habitación sea único en el sistema.
Logs de Debug
Para habilitar logs de debug:
pythonlogging.getLogger().setLevel(logging.DEBUG)
📈 Métricas y Monitoreo
El servicio está preparado para integración con herramientas de observabilidad:

Prometheus: Métricas de aplicación
Grafana: Dashboards de monitoreo
AWS CloudWatch: Métricas de infraestructura

🏷️ Versionado
Este proyecto utiliza SemVer para versionado:

MAJOR: Cambios incompatibles en API
MINOR: Nueva funcionalidad compatible
PATCH: Corrección de bugs

📄 Licencia
Este proyecto está licenciado bajo la Licencia MIT - ver el archivo LICENSE para más detalles.
👥 Equipo de Desarrollo

Desarrollador Principal: Tu Nombre
Curso: Programación Distribuida - 8vo Semestre
Universidad: Sistemas de Información

🆘 Soporte
Para reportar bugs o solicitar features:

Issues: Crear un issue en GitHub
Documentación: Consultar la documentación en /docs
Logs: Revisar los logs en /logs


Desarrollado con ❤️ para el proyecto final de Programación Distribuida