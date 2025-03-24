# **Prueba Técnica: API CRUD con Laravel y Docker**

## **Descripción**
Este proyecto es una API robusta desarrollada en Laravel para gestionar **Compañías**, **Contactos**, y **Notas Polimórficas**, diseñada para cumplir con requisitos de escalabilidad, organización y buenas prácticas. Utiliza **JWT (JSON Web Tokens)** para autenticación y está completamente dockerizado para facilitar su instalación y despliegue. Además, las relaciones polimórficas permiten una mayor flexibilidad al asociar notas con diferentes tipos de modelos.

---

## **Requerimientos del Sistema**
- **Docker** (Versiones recientes de Docker y Docker Compose).
- **Git** (Para clonar el repositorio).
- **PostgreSQL** (Configurado como servicio en Docker).
- **Composer** (Administrado automáticamente dentro del contenedor).

---

## **Características Principales**
1. **Autenticación JWT:**
   - Registro, inicio de sesión y cierre de sesión.
   - Protege todos los endpoints con tokens válidos.

2. **CRUD de Compañías:**
   - Operaciones para crear, leer, actualizar y eliminar compañías.
   - Relación **hasMany** con contactos.

3. **CRUD de Contactos:**
   - Operaciones para crear, leer, actualizar y eliminar contactos.
   - Relación **belongsTo** con compañías.

4. **Notas Polimórficas:**
   - Las notas pueden asociarse tanto a compañías como a contactos (y otros modelos en el futuro).
   - Operaciones CRUD completas.

5. **Dockerizado:**
   - Implementación portátil y eficiente mediante Docker Compose.

---

## **Instrucciones para la Instalación**

### **1. Clonar el repositorio**
Ejecuta el siguiente comando para clonar el proyecto:
```bash
git clone https://github.com/Jhormanarias/technical-test.git
```

### **2. Configurar el archivo `.env`**
Copia el archivo `.env.example` y renómbralo como `.env` tanto para la API como para la base de datos:
```bash
cp .env.example .env
```

Edita las variables del archivo `.env` y asegúrate de configurar las credenciales de la base de datos correctamente:
```dotenv
DB_CONNECTION=pgsql
DB_HOST=db
DB_PORT=5432
DB_DATABASE=prueba_tecnica
DB_USERNAME=postgres
DB_PASSWORD=tu_contraseña
JWT_SECRET=tu_clave_secreta_generada
```

### **3. Construir los contenedores con Docker**
Ejecuta el siguiente comando para construir los servicios definidos en `docker-compose.yml`:
```bash
docker-compose build
```

### **4. Levantar los contenedores**
Después de construir los contenedores, inicia el proyecto con:
```bash
docker-compose up -d
```

### **5. Instalar dependencias con Composer**
Accede al contenedor del servicio de la API para instalar las dependencias necesarias:
```bash
docker-compose exec api-novadeha bash
composer install
```

### **6. Ejecutar las migraciones**
Ejecuta las migraciones desde dentro del contenedor para configurar las tablas necesarias en la base de datos:
```bash
php artisan migrate
```

---

## **Ejecución del Proyecto**

Con los contenedores en funcionamiento, puedes acceder a la API a través de `http://localhost:8000`. Asegúrate de que los endpoints protegidos incluyan un token JWT válido en las cabeceras de la solicitud.

---

## **Uso de la API**
## **Endpoints Principales**

### **Autenticación**
- **Registro:** `POST /api/register`
- **Inicio de Sesión:** `POST /api/login`
- **Cierre de Sesión:** `POST /api/logout`

### **CRUD de Compañías**
1. **Listar compañías:** `GET /api/companies` (incluye contactos y notas).
2. **Crear una compañía:** `POST /api/companies`
3. **Consultar una compañía:** `GET /api/companies/{id}` (incluye relaciones).
4. **Actualizar una compañía:** `PUT /api/companies/{id}`
5. **Eliminar una compañía:** `DELETE /api/companies/{id}`

### **CRUD de Contactos**
1. **Listar contactos:** `GET /api/contacts` (incluye relaciones).
2. **Crear un contacto:** `POST /api/contacts`
3. **Consultar un contacto:** `GET /api/contacts/{id}`
4. **Actualizar un contacto:** `PUT /api/contacts/{id}`
5. **Eliminar un contacto:** `DELETE /api/contacts/{id}`

### **Notas Polimórficas**
1. **Crear una nota:** `POST /api/notes`
   ```json
   {
     "content": "Esta es una nota para la compañía.",
     "noteable_id": 1,
     "noteable_type": "App\\Models\\Company"
   }
   ```
2. **Listar todas las notas:** `GET /api/notes`
3. **Consultar una nota específica:** `GET /api/notes/{id}`
4. **Actualizar una nota:** `PUT /api/notes/{id}`
   ```json
   {
     "content": "Nota actualizada"
   }
   ```
5. **Eliminar una nota:** `DELETE /api/notes/{id}`

---

## **Estructura del Proyecto**

### **Controladores**
- **AuthController:** Gestiona la autenticación con JWT (registro, inicio y cierre de sesión).
- **CompanyController:** CRUD completo para compañías, con relaciones `contacts` y `notes`.
- **ContactController:** CRUD completo para contactos, con relaciones `company` y `notes`.
- **NoteController:** CRUD para notas polimórficas.

### **Modelos**
- **Company:**
  - Relación `hasMany` con `Contact`.
  - Relación `morphMany` con `Note`.
- **Contact:**
  - Relación `belongsTo` con `Company`.
  - Relación `morphMany` con `Note`.
- **Note:**
  - Relación polimórfica `morphTo` para asociarse a múltiples modelos.
- **Servicios**: Manejan la lógica de negocio (No está implementada).

---

### **Pruebas con Postman**
1. **Autenticación:**
   - Asegúrate de incluir el token JWT en las cabeceras:
     ```http
     Authorization: Bearer {jwt_token}
     ```
2. **CRUD de compañías y contactos:**
   - Prueba los endpoints para crear, leer, actualizar y eliminar compañías y contactos.
3. **Relaciones polimórficas:**
   - Crea notas para compañías y contactos y consulta sus relaciones.

---

## **Notas Técnicas**
- **Dockerización Completa:** El proyecto utiliza Docker para ejecutar la aplicación y la base de datos, lo que facilita la configuración en cualquier entorno.
- **Relaciones Polimórficas:** La tabla `notes` utiliza una relación polimórfica para asociarse con múltiples modelos, lo que la hace extensible y escalable.
- **Seguridad:** Todos los endpoints están protegidos mediante autenticación JWT.
- El contenedor de la base de datos persiste los datos en la carpeta `data/pg_data`.


## **Desarrollador**
Desarrollado por Jhorman Gañan Arias.

---
