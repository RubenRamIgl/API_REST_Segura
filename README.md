# Proyecto API REST Segura

## Planteamiento
La idea de la aplicación es desarrollar una **tienda de ropa** que maneje datos de la tienda, usuarios y productos. Para ello, se creará una base de datos con las tablas correspondientes. Dependiendo del rol del usuario, se podrá acceder a diferentes secciones y funcionalidades:
- **Clientes**: Podrán explorar productos y gestionar sus datos personales.
- **Administradores**: Tendrán acceso completo para gestionar usuarios, productos y tiendas.

## Clases

### Usuario
- `id` (Primary Key): `Long`
- `nombre`: `String`
- `apellido1`: `String`
- `apellido2`: `String`
- `edad`: `int`
- `direccion`: `String`
- `username`: `String` (nombre de usuario para acceder a la cuenta)
- `password`: `String` (almacenada de forma hasheada en la BD)
- `rol`: `String` (valores posibles: `Admin`, `Cliente`)

### Tienda
- `id` (Primary Key): `Long`
- `nombre`: `String`
- `ciudad`: `String`
- `direccion`: `String`
- `telefono`: `String`
- `productos`: `ArrayList<Producto>`

### Producto
- `id` (Primary Key): `Long`
- `nombre`: `String`
- `precio`: `float`
- `stock`: `int`

El sistema incluye un CRUD básico con restricciones de acceso en función del rol del usuario.

## Tablas

### Tabla `Usuarios`

| Columna      | Tipo de Dato   | Descripción                                           |  
|--------------|----------------|-------------------------------------------------------|  
| `id`         | `BIGINT`       | Clave primaria, identificador único del usuario.      |  
| `nombre`     | `VARCHAR(100)` | Nombre del usuario.                                   |  
| `apellido1`  | `VARCHAR(100)` | Primer apellido del usuario.                         |  
| `apellido2`  | `VARCHAR(100)` | Segundo apellido del usuario.                        |  
| `edad`       | `NUMBER`       | Edad del usuario.                                     |  
| `direccion`  | `VARCHAR(100)` | Dirección del usuario.                                |  
| `username`   | `VARCHAR(100)` | Nombre de usuario para acceder al sistema.           |  
| `password`   | `VARCHAR(100)` | Contraseña hasheada del usuario.                     |  
| `rol`        | `VARCHAR(20)`  | Rol del usuario (`Admin` o `Cliente`).               |  

### Tabla `Tiendas`

| Columna      | Tipo de Dato   | Descripción                                           |  
|--------------|----------------|-------------------------------------------------------|  
| `id`         | `BIGINT`       | Clave primaria, identificador único de la tienda.     |  
| `nombre`     | `VARCHAR(100)` | Nombre de la tienda.                                  |  
| `ciudad`     | `VARCHAR(100)` | Ciudad donde se encuentra la tienda.                 |  
| `direccion`  | `VARCHAR(100)`   | Dirección de la tienda.                               |  
| `telefono`   | `VARCHAR(20)`  | Número de teléfono de la tienda.                     |  
| `productos`  | `JSON`         | Lista de productos asociados a la tienda (opcional). |  

### Tabla `Productos`

| Columna      | Tipo de Dato     | Descripción                                           |  
|--------------|------------------|-------------------------------------------------------|  
| `id`         | `BIGINT`         | Clave primaria, identificador único del producto.     |  
| `nombre`     | `VARCHAR(100)`   | Nombre del producto.                                  |  
| `precio`     | `DECIMAL(10, 2)` | Precio del producto, con hasta 2 decimales.          |  
| `stock`      | `INT`            | Cantidad disponible del producto en stock.           |  


## Opciones por Rol

### Rol Administrador
El administrador tiene acceso completo a la gestión de usuarios, tiendas y productos.

1. **Usuarios**:
    - `GET`: Ver la lista completa de usuarios o un usuario específico.
    - `POST`: Crear nuevos usuarios (por ejemplo, registrar clientes).
    - `PUT`: Actualizar la información de un usuario.
    - `DELETE`: Eliminar usuarios.

2. **Tiendas**:
    - `GET`: Ver la lista de tiendas o una tienda específica.
    - `POST`: Crear nuevas tiendas.
    - `PUT`: Actualizar la información de una tienda.
    - `DELETE`: Eliminar tiendas.

3. **Productos**:
    - `GET`: Ver todos los productos o un producto específico.
    - `POST`: Añadir nuevos productos.
    - `PUT`: Modificar productos existentes (nombre, precio, stock).
    - `DELETE`: Eliminar productos.

### Rol Cliente
El cliente tiene permisos limitados, enfocados en el consumo de información y la gestión de su cuenta.

1. **Usuarios**:
    - `GET`: Ver su propia información de usuario.
    - `PUT`: Actualizar su propia información (nombre, apellidos, dirección, contraseña, etc.).
    - `DELETE`: Eliminar su propia cuenta.

2. **Tiendas**:
    - `GET`: Ver la lista de tiendas o una tienda específica.

3. **Productos**:
    - `GET`: Ver todos los productos o un producto específico, incluyendo el stock disponible.

## Endpoints

### Usuarios
| Método | Endpoint          | Rol           | Descripción                             |
|--------|-------------------|---------------|-----------------------------------------|
| GET    | `/usuarios`       | Admin         | Lista de todos los usuarios.            |
| GET    | `/usuarios/{id}`  | Admin/Cliente | Detalles de un usuario específico.      |
| POST   | `/usuarios`       | Admin/Público | Crear un nuevo usuario.                 |
| PUT    | `/usuarios/{id}`  | Admin/Cliente | Actualizar datos de un usuario.         |
| DELETE | `/usuarios/{id}`  | Admin/Cliente | Eliminar un usuario.                    |

### Tiendas
| Método | Endpoint          | Rol           | Descripción                             |
|--------|-------------------|---------------|-----------------------------------------|
| GET    | `/tiendas`        | Admin/Cliente | Lista de todas las tiendas.             |
| GET    | `/tiendas/{id}`   | Admin/Cliente | Detalles de una tienda específica.      |
| POST   | `/tiendas`        | Admin         | Crear una nueva tienda.                 |
| PUT    | `/tiendas/{id}`   | Admin         | Actualizar datos de una tienda.         |
| DELETE | `/tiendas/{id}`   | Admin         | Eliminar una tienda.                    |

### Productos
| Método | Endpoint           | Rol           | Descripción                             |
|--------|--------------------|---------------|-----------------------------------------|
| GET    | `/productos`       | Admin/Cliente | Lista de todos los productos.           |
| GET    | `/productos/{id}`  | Admin/Cliente | Detalles de un producto específico.     |
| POST   | `/productos`       | Admin         | Crear un nuevo producto.                |
| PUT    | `/productos/{id}`  | Admin         | Actualizar datos de un producto.        |
| DELETE | `/productos/{id}`  | Admin         | Eliminar un producto.                   |

## Descripción de los Endpoints

### Endpoints de Usuarios
- **GET `/usuarios`**: Retorna una lista de todos los usuarios en el sistema (solo para admin).
- **GET `/usuarios/{id}`**: Retorna los detalles de un usuario específico. Los administradores pueden acceder a cualquier usuario, mientras que los clientes solo pueden acceder a su propio perfil.
- **POST `/usuarios`**: Permite la creación de un nuevo usuario, como el registro de un cliente. Cualquier usuario público puede acceder a este endpoint.
- **PUT `/usuarios/{id}`**: Permite a los usuarios (clientes) o administradores modificar la información de un usuario (nombre, dirección, etc.).
- **DELETE `/usuarios/{id}`**: Permite a los usuarios (clientes) o administradores eliminar un usuario del sistema.

### Endpoints de Tiendas
- **GET `/tiendas`**: Retorna una lista de todas las tiendas disponibles. Los administradores y clientes pueden acceder a este endpoint.
- **GET `/tiendas/{id}`**: Muestra los detalles de una tienda específica.
- **POST `/tiendas`**: Permite la creación de una nueva tienda (solo administradores).
- **PUT `/tiendas/{id}`**: Permite actualizar los detalles de una tienda (solo administradores).
- **DELETE `/tiendas/{id}`**: Elimina una tienda del sistema (solo administradores).

### Endpoints de Productos
- **GET `/productos`**: Retorna una lista de todos los productos disponibles en el sistema.
- **GET `/productos/{id}`**: Muestra los detalles de un producto específico.
- **POST `/productos`**: Permite la creación de un nuevo producto (solo administradores).
- **PUT `/productos/{id}`**: Permite modificar los detalles de un producto (solo administradores).
- **DELETE `/productos/{id}`**: Elimina un producto del sistema (solo administradores).

## Lógica de Negocio

La aplicación tendrá una lógica de negocio que se enfoca en:
1. **Autenticación y Autorización**: Usar tokens JWT para autenticar y autorizar a los usuarios según su rol.
2. **Gestión de Usuarios**: Los administradores pueden crear, actualizar, ver y eliminar usuarios, mientras que los clientes solo pueden gestionar sus propios datos.
3. **Gestión de Productos y Tiendas**: Los administradores podrán gestionar los productos y tiendas en el sistema, mientras que los clientes solo podrán consultar la información.
4. **Restricciones de Seguridad**: Los roles de usuario se usarán para restringir el acceso a los recursos. Solo los administradores tendrán acceso completo.

## Excepciones y Códigos de Estado

Se implementarán las siguientes excepciones y códigos de estado:

- **404 Not Found**: Cuando el recurso solicitado no existe (usuario, tienda o producto no encontrado).
- **400 Bad Request**: Cuando la solicitud está mal formada o los datos enviados no son válidos.
- **401 Unauthorized**: Cuando el usuario no está autenticado correctamente.
- **403 Forbidden**: Cuando el usuario intenta acceder a un recurso para el cual no tiene permisos.
- **500 Internal Server Error**: Cuando ocurre un error inesperado en el servidor.

## Restricciones de Seguridad

1. **Autenticación**: Utilizaremos JWT (JSON Web Tokens) para la autenticación de usuarios. Cada usuario que inicie sesión recibirá un token que deberá incluir en el encabezado de las peticiones para acceder a los recursos protegidos.
2. **Roles y Permisos**: La API verificará el rol del usuario (Admin o Cliente) antes de permitir el acceso a ciertos endpoints.
3. **Contraseñas Hasheadas**: Las contraseñas de los usuarios se almacenarán de manera segura utilizando algoritmos de hash como bcrypt para evitar su exposición.
4. **Acceso Limitado a Endpoints**: Los clientes solo podrán acceder a información relacionada con su cuenta, mientras que los administradores tendrán acceso completo a todos los recursos.

## Requisitos
- Java
- IntelliJ IDEA
- Base de datos (MySQL)
- Framework para el desarrollo de API REST (Spring Boot)
- Sistema de autenticación (JWT)
