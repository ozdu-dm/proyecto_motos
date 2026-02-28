##  MotoClub App - Proyecto Final Web  ##

Aplicación web Full-Stack desarrollada en Python enfocada en la exhibición, comparación y reserva de pruebas de conducción de motos. 

En este proyecto se ha intentado hacer todo ha base del enunciado, cumpliendo con los requisitos de integración de bases de datos relacionales, autenticación de usuarios, herencia de plantillas HTML y diseño web responsivo.

------------------------------------------------------------------------------------------------

##  Características Principales  ##

El proyecto se divide en una parte pública y una zona privada exclusiva para usuarios registrados:

* ** Catálogo Público:** Visualización de la flota de motocicletas disponibles en la base de datos.
* ** Autenticación Segura:** Sistema completo de Registro y Login con encriptación de contraseñas.
* ** Comparador de Motos (Privado):** Herramienta para comparar las especificaciones de dos modelos diferentes.
* ** Sistema de Reservas (Privado):** Los usuarios pueden agendar una fecha y dejar su teléfono para realizar un *Test Ride* del modelo que elijan (Tercer formulario requerido).
* ** Panel de Usuario:** Sección "Mis Reservas" donde cada cliente puede consultar su historial de pruebas programadas.
* ** Portfolio Integrado:** Sección especial *About Me* que documenta y expone proyectos web anteriores (Gestor de Correos) usando capturas de pantalla reales.

------------------------------------------------------------------------------------------------

##  Tecnologías Utilizadas  ##


### Backend & Lógica de Servidor
* **Python 3:** Lenguaje de programación principal sobre el que corre toda la lógica de la aplicación.
* **Flask:** Micro-framework web utilizado para construir el servidor, definir las rutas (endpoints) y gestionar las peticiones HTTP de forma rápida y escalable.

### Base de Datos & Modelado
* **MySQL (XAMPP):** Sistema de Gestión de Bases de Datos Relacionales (RDBMS) utilizado para persistir la información de usuarios, el catálogo de motos y las reservas.
* **Flask-SQLAlchemy:** ORM (Object-Relational Mapper) que permite interactuar con MySQL mediante clases y objetos de Python. Facilita las consultas (CRUD), previene ataques de inyección SQL y genera las tablas automáticamente.
* **PyMySQL:** Driver (conector) que actúa como puente de comunicación nativo entre SQLAlchemy y el servidor MySQL de XAMPP.

### Seguridad & Autenticación
* **Flask-Login:** Librería encargada del manejo de sesiones de usuario. Controla las cookies del navegador y protege las rutas privadas (mediante el decorador `@login_required`).
* **Werkzeug (Security):** Herramienta criptográfica utilizada para generar hashes de contraseñas (`pbkdf2:sha256`) en la base de datos, garantizando que ninguna credencial se guarde en texto plano.

### Frontend & Interfaz de Usuario
* **Jinja2:** Motor de plantillas integrado en Flask. Permite la **herencia de plantillas HTML** (uso de un `base.html` maestro) y la renderización dinámica de datos mediante estructuras de control (bucles `for`, condicionales `if`) directamente en el HTML.
* **Bootstrap 5:** Framework CSS y JS utilizado para construir una interfaz de usuario moderna, limpia y **100% responsive** (adaptable a móviles y tablets). Se hace uso de su sistema de rejilla (Grid), tarjetas (Cards), botones y alertas.
* **HTML5:** Lenguaje de marcado estándar para la estructuración semántica de la web.

------------------------------------------------------------------------------------------------


## 📂 Estructura del Proyecto

```text
proyecto_motos/
│
├── app.py                 # Código principal del backend, modelos y rutas
├── README.md              # Documentación técnica del proyecto
│
├── static/                # Archivos estáticos
│   ├── addmail.PNG        # Captura portfolio 1
│   └── get_mail.PNG       # Captura portfolio 2
│
└── templates/             # Vistas y plantillas HTML (Frontend)
    ├── base.html          # Plantilla maestra (Navbar y estructura base)
    ├── index.html         # Inicio / Catálogo público
    ├── login.html         # Formulario de inicio de sesión
    ├── register.html      # Formulario de registro seguro
    ├── compare.html       # Comparador de motocicletas
    ├── reservar.html      # Formulario de agendamiento de prueba (Test Ride)
    ├── mis_reservas.html  # Historial privado del usuario
    └── about.html         # Portfolio personal y proyectos previos
