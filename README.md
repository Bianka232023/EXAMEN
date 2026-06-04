# EXAMEN
PROGRAMACION V
# Nova: Space Expedition Mission Control

**Nova** es un sistema web premium de gestión de información (CRUD) diseñado para monitorear y documentar misiones de exploración espacial y robótica extraplanetaria. El sistema consta de una interfaz de consola de comando futurista y responsiva (Frontend) y una API RESTful robusta (Backend) conectada a una base de datos MongoDB.

Desarrollado para el **Examen Parcial de Desarrollo de Aplicaciones Web** por **Vasquez Bianka**.

---

## 🛠️ Tecnologías y Estructura

El proyecto está diseñado bajo una arquitectura modular y limpia, separando las responsabilidades de datos, rutas, lógica de control y presentación visual:

*   **Backend**: Node.js, Express, MongoDB (Mongoose).
*   **Frontend**: HTML5, CSS3 personalizado (Glassmorphism & Neon Glow), JavaScript ES6 (Fetch API, async/await), Bootstrap 5 (Grilla responsiva e iconos).

### Estructura de Carpetas

```text
nova-space-missions/
├── backend/
│   ├── config/
│   │   └── db.js            # Módulo de conexión a MongoDB
│   ├── controllers/
│   │   └── missionController.js # Lógica de negocio (Controladores CRUD)
│   ├── models/
│   │   └── Mission.js       # Modelo/Esquema de Mongoose (Misión Espacial)
│   ├── routes/
│   │   └── missionRoutes.js # Definición de endpoints de la API REST
│   ├── .env                 # Variables de entorno
│   ├── package.json         # Dependencias de npm (Express, Mongoose, etc.)
│   └── server.js            # Archivo de inicio del servidor Express
├── frontend/
│   ├── css/
│   │   └── styles.css       # Estilos estéticos de consola espacial
│   ├── js/
│   │   └── app.js           # Controlador JS cliente (Llamadas asíncronas y DOM)
│   └── index.html           # Interfaz de usuario (Bootstrap responsivo)
├── README.md                # Guía de instalación y documentación
└── .gitignore               # Exclusión de node_modules y entornos locales
```

---

## 💾 Requisitos Previos

Para ejecutar esta aplicación localmente necesitarás:
1.  [Node.js](https://nodejs.org/) (versión 16.x o superior).
2.  [MongoDB](https://www.mongodb.com/try/download/community) instalado y ejecutándose de forma local (en el puerto estándar `27017`), o una base de datos en la nube mediante MongoDB Atlas.

---

## 🚀 Instrucciones de Instalación y Ejecución

### 1. Clonar o descomprimir el proyecto
Extrae el contenido de `Vasquez_Bianka_Examen_Parcial.zip` en tu directorio local de desarrollo.

### 2. Configurar el Backend
Navega a la carpeta del backend e instala las dependencias necesarias:
```bash
cd backend
npm install
```

### 3. Modificar Variables de Entorno (Opcional)
Se ha incluido un archivo `.env` pre-configurado para conectar con una instancia de MongoDB local. Si necesitas conectar con MongoDB Atlas, modifica la variable `MONGO_URI` dentro del archivo `backend/.env`:
```env
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/nova-missions
NODE_ENV=development
```

### 4. Iniciar el Servidor de Backend
Puedes ejecutar el servidor en modo de desarrollo (utiliza `nodemon` para reiniciarse automáticamente ante cualquier cambio de código) o en modo de producción estándar:

*   **Modo de Desarrollo**:
    ```bash
    npm run dev
    ```
*   **Modo Estándar**:
    ```bash
    npm start
    ```

El backend se iniciará e imprimirá en consola:
```text
[Database] Attempting to connect to: mongodb://127.0.0.1:27017/nova-missions
[Server] Command Console running in development mode on port 5000
[Server] Access URL: http://localhost:5000
[Database] MongoDB Connected: 127.0.0.1
```

### 5. Lanzar el Frontend
El backend de Express está configurado para servir los archivos del frontend estáticamente. Puedes probar el sistema completo accediendo directamente a:
👉 **[http://localhost:5000](http://localhost:5000)** en tu navegador web.

*(Nota: También puedes abrir el archivo `frontend/index.html` directamente en tu navegador o utilizando el Live Server de VS Code; el script cliente se adaptará automáticamente para consultar a `http://localhost:5000`)*.

---

## 📡 Documentación de Rutas de la API (RESTful)

Todos los endpoints del recurso `Mission` (Misiones Espaciales) están prefijados con `/api/missions`.

| Método | Endpoint | Descripción | Body Requerido (JSON) | Códigos de Estado Devueltos |
| :--- | :--- | :--- | :--- | :--- |
| **GET** | `/api/missions` | Obtiene la lista completa de misiones ordenadas por fecha de creación (de la más reciente a la más antigua). | *Ninguno* | **200 OK**: Retorna lista de misiones.<br>**500 Server Error**: Problemas en el servidor. |
| **GET** | `/api/missions/:id` | Obtiene el detalle de una misión espacial específica por su ID de MongoDB. | *Ninguno* | **200 OK**: Retorna el objeto de la misión.<br>**400 Bad Request**: Formato de ID inválido.<br>**404 Not Found**: Misión no encontrada. |
| **POST** | `/api/missions` | Registra una nueva misión espacial en la base de datos con las validaciones de esquema. | Objeto con datos de la misión (ver abajo). | **201 Created**: Registro exitoso.<br>**400 Bad Request**: Validación fallida o campos faltantes.<br>**500 Server Error**: Error en base de datos. |
| **PUT** | `/api/missions/:id` | Modifica los datos de una misión espacial existente y revalida la estructura. | Objeto con campos a actualizar. | **200 OK**: Modificación exitosa.<br>**400 Bad Request**: Formato de ID o campos inválidos.<br>**404 Not Found**: Misión no encontrada. |
| **DELETE** | `/api/missions/:id` | Elimina definitivamente el registro de una misión espacial por su ID. | *Ninguno* | **200 OK**: Eliminación exitosa.<br>**400 Bad Request**: Formato de ID inválido.<br>**404 Not Found**: Misión no encontrada. |

### Modelo de Datos (Esquema del Recurso JSON)

Al crear (**POST**) o actualizar (**PUT**) una misión, se debe proveer el siguiente formato JSON en el cuerpo de la petición:

```json
{
  "title": "Rover Expedition Ares III",
  "destination": "Marte (Crater Jezero)",
  "commander": "Dr. Helena Vance",
  "launchDate": "2026-08-12",
  "status": "In Progress",
  "budget": 45000000,
  "description": "Despliegue del explorador solar con el fin de perforar regolito profundo y localizar reservas de hielo de agua."
}
```

*   **`title`**: Cadena de texto (Requerido).
*   **`destination`**: Cadena de texto (Requerido).
*   **`commander`**: Cadena de texto (Requerido).
*   **`launchDate`**: Tipo Fecha (Requerido).
*   **`status`**: Cadena de texto restringida a: `Planned`, `In Progress`, `Completed`, o `Aborted` (Requerido).
*   **`budget`**: Tipo Número mayor o igual a 0 (Requerido).
*   **`description`**: Cadena de texto (Requerido).

---

## ⚡ Pruebas y Validación Manual (Checklist)

1.  **Listar Registros (GET)**: Al cargar el panel web, se listan todas las misiones en una cuadrícula responsiva de tarjetas con un efecto de transición suave. Los indicadores de telemetría de la barra superior recalculan los conteos y suman el presupuesto global.
2.  **Filtrar y Buscar**: Puedes escribir en la barra de búsqueda superior o cambiar el selector de estados para filtrar localmente en tiempo real (instantáneo) sobre la interfaz.
3.  **Crear Registros (POST)**: Completa el formulario de "Nueva Misión" y haz clic en "Registrar Misión". Si dejas campos vacíos o usas presupuestos negativos, el sistema mostrará alertas en tiempo real de validación de HTML5 y Express. Al crearse, se emite una notificación de telemetría holográfica flotante de éxito y el panel se actualiza automáticamente.
4.  **Editar Registros (PUT)**: Haz clic en "Editar" en cualquier misión para rellenar un cuadro modal de edición con los datos actuales. Al guardar los cambios, se actualiza el documento en el servidor de forma reactiva.
5.  **Eliminar Registros (DELETE)**: Haz clic en "Purgar". Un modal de seguridad evitará eliminaciones accidentales. Al confirmar, se purga de la base de datos y se recalculan las estadísticas de la consola.
