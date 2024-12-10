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

## Requisitos
- Java
- IntelliJ IDEA
- Base de datos (MySQL)
- Framework para el desarrollo de API REST (Spring Boot)
- Sistema de autenticación (JWT)
