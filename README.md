## Documentación de APIs

El sistema Eventon proporciona las siguientes APIs para gestionar eventos, categorías, etiquetas, organizadores y autenticación de usuarios.

### Índice de APIs
- [Autenticación](#autenticación)
  - [Iniciar sesión](#iniciar-sesión)
  - [Registrar usuario](#registrar-usuario)
  - [Recuperar contraseña](#recuperar-contraseña)
  - [Restablecer contraseña](#restablecer-contraseña)
- [Eventos](#eventos)
  - [Obtener todos los eventos](#obtener-todos-los-eventos)
  - [Filtrar eventos](#filtrar-eventos)
  - [Obtener un evento por ID](#obtener-un-evento-por-id)
- [Categorías](#categorías)
  - [Obtener todas las categorías](#obtener-todas-las-categorías)
  - [Filtrar categorías](#filtrar-categorías)
  - [Obtener una categoría por ID](#obtener-una-categoría-por-id)
- [Etiquetas](#etiquetas)
  - [Obtener todas las etiquetas](#obtener-todas-las-etiquetas)
  - [Filtrar etiquetas](#filtrar-etiquetas)
  - [Obtener una etiqueta por ID](#obtener-una-etiqueta-por-id)
- [Lugares](#lugares)
  - [Obtener todos los lugares](#obtener-todos-los-lugares)
  - [Filtrar lugares](#filtrar-lugares)
  - [Obtener un lugar por ID](#obtener-un-lugar-por-id)
- [Organizadores](#organizadores)
  - [Obtener todos los organizadores](#obtener-todos-los-organizadores)
  - [Filtrar organizadores](#filtrar-organizadores)
  - [Obtener un organizador por ID](#obtener-un-organizador-por-id)
- [Subida de archivos](#subida-de-archivos)
  - [Subir un archivo](#subir-un-archivo)

### Autenticación

#### Iniciar sesión
- **Endpoint**: `/api/auth/login`
- **Método**: POST
- **Descripción**: Autentica a un usuario y devuelve un token JWT.
- **Parámetros**:
  ```json
  {
    "email": "usuario@ejemplo.com",
    "password": "contraseña"
  }
  ```
- **Respuesta exitosa**:
  ```json
  {
    "user": {
      "id": 1,
      "nombre": "Nombre Usuario",
      "nickname": "usuario",
      "email": "usuario@ejemplo.com",
      "rango": "admin"
    },
    "token": "jwt_token_aquí"
  }
  ```
- **Respuesta de error**:
  ```json
  {
    "error": "Credenciales inválidas"
  }
  ```

#### Registrar usuario
- **Endpoint**: `/api/auth/register`
- **Método**: POST
- **Descripción**: Registra un nuevo usuario en el sistema.
- **Parámetros**:
  ```json
  {
    "nombre": "Nombre Completo",
    "nickname": "apodo_usuario",
    "email": "usuario@ejemplo.com",
    "password": "contraseña"
  }
  ```
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "message": "Usuario registrado correctamente",
    "user": {
      "id": 1,
      "nombre": "Nombre Completo",
      "nickname": "apodo_usuario",
      "email": "usuario@ejemplo.com",
      "rango": "usuario"
    }
  }
  ```
- **Respuesta de error**:
  ```json
  {
    "success": false,
    "message": "El email ya está registrado"
  }
  ```

#### Recuperar contraseña
- **Endpoint**: `/api/auth/forgot-password`
- **Método**: POST
- **Descripción**: Envía un correo electrónico con un enlace para restablecer la contraseña.
- **Parámetros**:
  ```json
  {
    "email": "usuario@ejemplo.com"
  }
  ```
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "message": "Se ha enviado un correo electrónico con instrucciones para restablecer la contraseña"
  }
  ```

#### Restablecer contraseña
- **Endpoint**: `/api/auth/reset-password`
- **Método**: POST
- **Descripción**: Restablece la contraseña de un usuario utilizando un token válido.
- **Parámetros**:
  ```json
  {
    "token": "token_de_restablecimiento",
    "password": "nueva_contraseña"
  }
  ```
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "message": "Contraseña actualizada correctamente"
  }
  ```

### Eventos

#### Obtener todos los eventos
- **Endpoint**: `/api/eventos/lista`
- **Método**: GET
- **Descripción**: Obtiene la lista de todos los eventos publicados con información detallada de lugar, organizador y etiquetas.
- **Respuesta exitosa**:
  ```json
  {
    "eventos": [
      {
        "id": 1,
        "titulo": "Nombre del evento",
        "subtitulo": "Subtítulo del evento",
        "fecha_inicio": "2025-04-15T18:00:00.000Z",
        "fecha_fin": "2025-04-15T22:00:00.000Z",
        "duracion": "4 horas",
        "categoria": "1",
        "descripcion": "Descripción corta",
        "descripcion_larga": "Descripción detallada",
        "imagen_principal": "/uploads/eventos/imagen.jpg",
        "precio": 100.00,
        "estado": "publicado",
        "url": "https://ejemplo.com/evento",
        "tipo_destacado": "oro",
        "lugar": {
          "id": 1,
          "nombre": "Nombre del lugar",
          "direccion": "Dirección",
          "ciudad": "Ciudad",
          "provincia": "Provincia",
          "codigo_postal": "12345",
          "coordenadas_lat": 12.3456,
          "coordenadas_lng": -12.3456,
          "capacidad": 500,
          "instrucciones_acceso": "Instrucciones"
        },
        "organizador": {
          "id": 1,
          "nombre": "Nombre del organizador",
          "descripcion": "Descripción",
          "seguidores": 1000,
          "email": "organizador@ejemplo.com",
          "telefono": "123456789",
          "website": "https://organizador.com",
          "fecha_creacion": "2025-01-01T00:00:00.000Z"
        },
        "etiquetas": [
          {
            "id": 1,
            "nombre": "Etiqueta 1"
          },
          {
            "id": 2,
            "nombre": "Etiqueta 2"
          }
        ]
      }
    ]
  }
  ```

#### Filtrar eventos
- **Endpoint**: `/api/eventos/filtrar`
- **Método**: GET
- **Descripción**: Filtra eventos según varios criterios.
- **Parámetros de consulta**:
  - `titulo`: Filtra por título (búsqueda parcial)
  - `subtitulo`: Filtra por subtítulo (búsqueda parcial)
  - `descripcion`: Filtra por descripción (búsqueda parcial)
  - `estado`: Filtra por estado (`borrador`, `publicado`, `cancelado`, `finalizado`)
  - `organizador_id`: Filtra por ID del organizador
  - `lugar_id`: Filtra por ID del lugar
  - `categoria`: Filtra por ID de la categoría
  - `etiqueta_id`: Filtra por ID de la etiqueta
  - `precio_min`: Filtra por precio mínimo
  - `precio_max`: Filtra por precio máximo
  - `tipo_destacado`: Filtra por tipo destacado (`normal`, `oro`, `plata`, `bronce`)
  - `fecha_inicio_desde`: Filtra por fecha de inicio desde (formato YYYY-MM-DD)
  - `fecha_inicio_hasta`: Filtra por fecha de inicio hasta (formato YYYY-MM-DD)
  - `fecha_fin_desde`: Filtra por fecha de fin desde (formato YYYY-MM-DD)
  - `fecha_fin_hasta`: Filtra por fecha de fin hasta (formato YYYY-MM-DD)
  - `ciudad`: Filtra por ciudad del lugar (búsqueda parcial)
  - `provincia`: Filtra por provincia del lugar (búsqueda parcial)
  - `busqueda`: Búsqueda general en título, subtítulo, descripción, nombre del organizador y nombre del lugar
  - `ordenar_por`: Campo por el cual ordenar los resultados (opciones: 'titulo', 'fecha_inicio', 'fecha_fin', 'precio', 'fecha_creacion', 'fecha_actualizacion')
  - `orden`: Dirección del ordenamiento ('ASC' o 'DESC')
  - `limite`: Número máximo de resultados por página (por defecto: 50)
  - `pagina`: Número de página para paginación (por defecto: 1)
- **Ejemplos de uso**:
  ```bash
  # Filtrar eventos por categoría y rango de precios
  curl -X GET "http://localhost:3000/api/eventos/filtrar?categoria=1&precio_min=50&precio_max=200"
  
  # Buscar eventos en una ciudad específica con una palabra clave
  curl -X GET "http://localhost:3000/api/eventos/filtrar?ciudad=BuenosAires&busqueda=concierto"
  
  # Filtrar eventos futuros ordenados por fecha
  curl -X GET "http://localhost:3000/api/eventos/filtrar?fecha_inicio_desde=2025-05-01T00:00:00.000Z&ordenar_por=fecha_inicio&orden=ASC"
  
  # Filtrar eventos de un organizador específico con paginación
  curl -X GET "http://localhost:3000/api/eventos/filtrar?organizador_id=5&pagina=2&limite=10"
  ```
- **Casos de uso**:
  - Implementar búsqueda avanzada de eventos
  - Filtrar eventos por ubicación geográfica
  - Mostrar eventos por rango de precios
  - Listar eventos de un organizador específico
  - Encontrar eventos con etiquetas específicas
  - Implementar paginación para grandes conjuntos de resultados
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "eventos": [
      {
        "id": 1,
        "titulo": "Nombre del evento",
        "subtitulo": "Subtítulo del evento",
        "fecha_inicio": "2025-04-15T18:00:00.000Z",
        "fecha_fin": "2025-04-15T22:00:00.000Z",
        "duracion": "4 horas",
        "categoria": "1",
        "descripcion": "Descripción corta",
        "descripcion_larga": "Descripción detallada",
        "imagen_principal": "/uploads/eventos/imagen.jpg",
        "precio": 100.00,
        "estado": "publicado",
        "url": "https://ejemplo.com/evento",
        "tipo_destacado": "oro",
        "lugar": {
          "id": 1,
          "nombre": "Nombre del lugar",
          "direccion": "Dirección",
          "ciudad": "Ciudad",
          "provincia": "Provincia",
          "codigo_postal": "12345",
          "coordenadas_lat": 12.3456,
          "coordenadas_lng": -12.3456,
          "capacidad": 500,
          "instrucciones_acceso": "Instrucciones"
        },
        "organizador": {
          "id": 1,
          "nombre": "Nombre del organizador",
          "descripcion": "Descripción",
          "seguidores": 1000,
          "email": "organizador@ejemplo.com",
          "telefono": "123456789",
          "website": "https://organizador.com",
          "fecha_creacion": "2025-01-01T00:00:00.000Z"
        },
        "etiquetas": [
          {
            "id": 1,
            "nombre": "Etiqueta 1"
          },
          {
            "id": 2,
            "nombre": "Etiqueta 2"
          }
        ]
      }
    ],
    "paginacion": {
      "total": 42,
      "pagina": 1,
      "limite": 50,
      "paginas": 1
    }
  }
  ```
- **Respuestas de error**:
  - Error del servidor (500):
    ```json
    {
      "success": false,
      "message": "Error al filtrar eventos: [descripción del error]"
    }
    ```
- **Notas**:
  - Esta API mantiene la misma estructura de respuesta que la API de listar eventos, añadiendo información de paginación
  - Se pueden combinar múltiples parámetros para realizar filtrados complejos
  - Si no se especifica ningún filtro, se devolverán todos los eventos
  - El parámetro `busqueda` realiza una búsqueda general en varios campos relevantes


### Categorías

#### Obtener todas las categorías
- **Endpoint**: `/api/categorias`
- **Método**: GET
- **Descripción**: Obtiene la lista de todas las categorías disponibles.
- **Parámetros de consulta**:
  - `busqueda`: Filtro opcional para buscar categorías por nombre
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "categorias": [
      {
        "id": 1,
        "nombre": "Categoría 1",
        "icon": "icon-categoria-1"
      }
    ]
  }
  ```

#### Filtrar categorías
- **Endpoint**: `/api/categorias/filtrar`
- **Método**: GET
- **Descripción**: Obtiene una lista filtrada de categorías según múltiples criterios.
- **Parámetros de consulta**:
  - `id`: Filtra por ID de la categoría
  - `nombre`: Filtra por nombre de la categoría (búsqueda parcial)
  - `icon`: Filtra por nombre del icono (búsqueda parcial)
  - `busqueda`: Búsqueda general en nombre e icono
  - `ordenar_por`: Campo por el cual ordenar los resultados (opciones: 'id', 'nombre', 'icon')
  - `orden`: Dirección del ordenamiento ('ASC' o 'DESC')
  - `limite`: Número máximo de resultados por página (por defecto: 50)
  - `pagina`: Número de página para paginación (por defecto: 1)
- **Ejemplos de uso**:
  ```bash
  # Filtrar categorías por nombre
  curl -X GET "http://localhost:3000/api/categorias/filtrar?nombre=Concierto"
  
  # Buscar categorías con un icono específico
  curl -X GET "http://localhost:3000/api/categorias/filtrar?icon=music"
  
  # Obtener categorías ordenadas por nombre descendente
  curl -X GET "http://localhost:3000/api/categorias/filtrar?ordenar_por=nombre&orden=DESC"
  
  # Paginación de resultados
  curl -X GET "http://localhost:3000/api/categorias/filtrar?pagina=2&limite=10"
  ```
- **Casos de uso**:
  - Implementar búsqueda avanzada de categorías
  - Mostrar categorías con paginación
  - Ordenar categorías por diferentes criterios
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "categorias": [
      {
        "id": 1,
        "nombre": "Categoría 1",
        "icon": "icon-categoria-1"
      }
    ],
    "paginacion": {
      "total": 25,
      "pagina": 1,
      "limite": 50,
      "paginas": 1
    }
  }
  ```
- **Respuestas de error**:
  - Error del servidor (500):
    ```json
    {
      "success": false,
      "message": "Error al filtrar categorías: [descripción del error]"
    }
    ```

#### Obtener una categoría por ID
- **Endpoint**: `/api/categorias/[id]`
- **Método**: GET
- **Descripción**: Obtiene una categoría específica por su ID.
- **Parámetros de ruta**:
  - `id`: Identificador único de la categoría (número entero)
- **Ejemplo de uso**:
  ```bash
  # Obtener la categoría con ID 1
  curl -X GET "http://localhost:3000/api/categorias/1"
  ```
- **Casos de uso**:
  - Mostrar detalles de una categoría específica
  - Verificar la existencia de una categoría
  - Obtener información para editar una categoría
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "categoria": {
      "id": 1,
      "nombre": "Categoría 1",
      "icon": "icon-categoria-1"
    }
  }
  ```
- **Respuestas de error**:
  - Categoría no encontrada (404):
    ```json
    {
      "success": false,
      "message": "Categoría no encontrada"
    }
    ```
  - Error del servidor (500):
    ```json
    {
      "success": false,
      "message": "Error al obtener categoría: [descripción del error]"
    }
    ```


### Etiquetas

#### Obtener todas las etiquetas
- **Endpoint**: `/api/etiquetas`
- **Método**: GET
- **Descripción**: Obtiene la lista de todas las etiquetas disponibles.
- **Parámetros de consulta**:
  - `busqueda`: Filtro opcional para buscar etiquetas por nombre
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "etiquetas": [
      {
        "id": 1,
        "nombre": "Etiqueta 1"
      }
    ]
  }
  ```

#### Filtrar etiquetas
- **Endpoint**: `/api/etiquetas/filtrar`
- **Método**: GET
- **Descripción**: Obtiene una lista filtrada de etiquetas según múltiples criterios.
- **Parámetros de consulta**:
  - `id`: Filtra por ID de la etiqueta
  - `nombre`: Filtra por nombre de la etiqueta (búsqueda parcial)
  - `evento_id`: Filtra etiquetas asociadas a un evento específico
  - `busqueda`: Búsqueda general en el nombre de la etiqueta
  - `ordenar_por`: Campo por el cual ordenar los resultados (opciones: 'id', 'nombre')
  - `orden`: Dirección del ordenamiento ('ASC' o 'DESC')
  - `limite`: Número máximo de resultados por página (por defecto: 50)
  - `pagina`: Número de página para paginación (por defecto: 1)
- **Ejemplos de uso**:
  ```bash
  # Filtrar etiquetas por nombre
  curl -X GET "http://localhost:3000/api/etiquetas/filtrar?nombre=musica"
  
  # Buscar etiquetas asociadas a un evento específico
  curl -X GET "http://localhost:3000/api/etiquetas/filtrar?evento_id=5"
  
  # Obtener etiquetas ordenadas por nombre descendente
  curl -X GET "http://localhost:3000/api/etiquetas/filtrar?ordenar_por=nombre&orden=DESC"
  
  # Paginación de resultados
  curl -X GET "http://localhost:3000/api/etiquetas/filtrar?pagina=2&limite=10"
  ```
- **Casos de uso**:
  - Implementar búsqueda avanzada de etiquetas
  - Mostrar etiquetas asociadas a un evento específico
  - Ordenar etiquetas por diferentes criterios
  - Implementar paginación para grandes conjuntos de resultados
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "etiquetas": [
      {
        "id": 1,
        "nombre": "Etiqueta 1"
      }
    ],
    "paginacion": {
      "total": 25,
      "pagina": 1,
      "limite": 50,
      "paginas": 1
    }
  }
  ```
- **Respuestas de error**:
  - Error del servidor (500):
    ```json
    {
      "success": false,
      "message": "Error al filtrar etiquetas: [descripción del error]"
    }
    ```

#### Obtener una etiqueta por ID
- **Endpoint**: `/api/etiquetas/[id]`
- **Método**: GET
- **Descripción**: Obtiene una etiqueta específica por su ID.
- **Parámetros de ruta**:
  - `id`: Identificador único de la etiqueta (número entero)
- **Ejemplo de uso**:
  ```bash
  # Obtener la etiqueta con ID 1
  curl -X GET "http://localhost:3000/api/etiquetas/1"
  ```
- **Casos de uso**:
  - Mostrar detalles de una etiqueta específica
  - Verificar la existencia de una etiqueta
  - Obtener información para editar una etiqueta
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "etiqueta": {
      "id": 1,
      "nombre": "Etiqueta 1"
    }
  }
  ```
- **Respuestas de error**:
  - Etiqueta no encontrada (404):
    ```json
    {
      "success": false,
      "message": "Etiqueta no encontrada"
    }
    ```
  - Error del servidor (500):
    ```json
    {
      "success": false,
      "message": "Error al obtener etiqueta: [descripción del error]"
    }
    ```

### Lugares

#### Obtener todos los lugares
- **Endpoint**: `/api/lugares`
- **Método**: GET
- **Descripción**: Obtiene la lista de todos los lugares disponibles.
- **Parámetros de consulta**:
  - `busqueda`: Filtro opcional para buscar lugares por nombre, dirección, ciudad o provincia
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "lugares": [
      {
        "id": 1,
        "nombre": "Nombre del lugar",
        "direccion": "Dirección del lugar",
        "ciudad": "Ciudad",
        "provincia": "Provincia",
        "codigo_postal": "12345",
        "coordenadas_lat": 12.3456,
        "coordenadas_lng": -12.3456,
        "capacidad": 500,
        "instrucciones_acceso": "Instrucciones para acceder al lugar"
      }
    ]
  }
  ```

#### Filtrar lugares
- **Endpoint**: `/api/lugares/filtrar`
- **Método**: GET
- **Descripción**: Obtiene una lista filtrada de lugares según múltiples criterios.
- **Parámetros de consulta**:
  - `id`: Filtra por ID del lugar
  - `nombre`: Filtra por nombre del lugar (búsqueda parcial)
  - `direccion`: Filtra por dirección del lugar (búsqueda parcial)
  - `ciudad`: Filtra por ciudad del lugar (búsqueda parcial)
  - `provincia`: Filtra por provincia del lugar (búsqueda parcial)
  - `codigo_postal`: Filtra por código postal del lugar (búsqueda parcial)
  - `capacidad`: Filtra lugares con al menos esta capacidad
  - `busqueda`: Búsqueda general en nombre, dirección, ciudad y provincia
  - `ordenar_por`: Campo por el cual ordenar los resultados (opciones: 'id', 'nombre', 'direccion', 'ciudad', 'provincia', 'codigo_postal', 'capacidad')
  - `orden`: Dirección del ordenamiento ('ASC' o 'DESC')
  - `limite`: Número máximo de resultados por página (por defecto: 50)
  - `pagina`: Número de página para paginación (por defecto: 1)
- **Ejemplos de uso**:
  ```bash
  # Filtrar lugares por nombre
  curl -X GET "http://localhost:3000/api/lugares/filtrar?nombre=teatro"
  
  # Buscar lugares en una ciudad específica
  curl -X GET "http://localhost:3000/api/lugares/filtrar?ciudad=Buenos Aires"
  
  # Obtener lugares con capacidad mínima de 200 personas
  curl -X GET "http://localhost:3000/api/lugares/filtrar?capacidad=200"
  
  # Paginación de resultados
  curl -X GET "http://localhost:3000/api/lugares/filtrar?pagina=2&limite=10"
  ```
- **Casos de uso**:
  - Implementar búsqueda avanzada de lugares
  - Filtrar lugares por ubicación geográfica
  - Encontrar lugares con capacidad suficiente para un evento
  - Implementar paginación para grandes conjuntos de resultados
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "lugares": [
      {
        "id": 1,
        "nombre": "Nombre del lugar",
        "direccion": "Dirección del lugar",
        "ciudad": "Ciudad",
        "provincia": "Provincia",
        "codigo_postal": "12345",
        "coordenadas_lat": 12.3456,
        "coordenadas_lng": -12.3456,
        "capacidad": 500,
        "instrucciones_acceso": "Instrucciones para acceder al lugar"
      }
    ],
    "paginacion": {
      "total": 25,
      "pagina": 1,
      "limite": 50,
      "paginas": 1
    }
  }
  ```
- **Respuestas de error**:
  - Error del servidor (500):
    ```json
    {
      "success": false,
      "message": "Error al filtrar lugares: [descripción del error]"
    }
    ```

#### Obtener un lugar por ID
- **Endpoint**: `/api/lugares/[id]`
- **Método**: GET
- **Descripción**: Obtiene un lugar específico por su ID.
- **Parámetros de ruta**:
  - `id`: Identificador único del lugar (número entero)
- **Ejemplo de uso**:
  ```bash
  # Obtener el lugar con ID 1
  curl -X GET "http://localhost:3000/api/lugares/1"
  ```
- **Casos de uso**:
  - Mostrar detalles de un lugar específico
  - Verificar la existencia de un lugar
  - Obtener información para editar un lugar
  - Mostrar la ubicación de un evento
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "lugar": {
      "id": 1,
      "nombre": "Nombre del lugar",
      "direccion": "Dirección del lugar",
      "ciudad": "Ciudad",
      "provincia": "Provincia",
      "codigo_postal": "12345",
      "coordenadas_lat": 12.3456,
      "coordenadas_lng": -12.3456,
      "capacidad": 500,
      "instrucciones_acceso": "Instrucciones para acceder al lugar"
    }
  }
  ```
- **Respuestas de error**:
  - Lugar no encontrado (404):
    ```json
    {
      "success": false,
      "message": "Lugar no encontrado"
    }
    ```
  - Error del servidor (500):
    ```json
    {
      "success": false,
      "message": "Error al obtener lugar: [descripción del error]"
    }
    ```

### Organizadores

#### Obtener todos los organizadores
- **Endpoint**: `/api/organizadores`
- **Método**: GET
- **Descripción**: Obtiene la lista de todos los organizadores disponibles.
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "data": [
      {
        "id": 1,
        "nombre": "Nombre del organizador"
      }
    ]
  }
  ```

#### Filtrar organizadores
- **Endpoint**: `/api/organizadores/filtrar`
- **Método**: GET
- **Descripción**: Obtiene una lista filtrada de organizadores según múltiples criterios.
- **Parámetros de consulta**:
  - `id`: Filtra por ID del organizador
  - `nombre`: Filtra por nombre del organizador (búsqueda parcial)
  - `email`: Filtra por email del organizador (búsqueda parcial)
  - `telefono`: Filtra por teléfono del organizador (búsqueda parcial)
  - `website`: Filtra por sitio web del organizador (búsqueda parcial)
  - `seguidores_min`: Filtra organizadores con al menos este número de seguidores
  - `seguidores_max`: Filtra organizadores con no más de este número de seguidores
  - `busqueda`: Búsqueda general en nombre, descripción, email, teléfono y sitio web
  - `ordenar_por`: Campo por el cual ordenar los resultados (opciones: 'id', 'nombre', 'seguidores', 'email', 'fecha_creacion')
  - `orden`: Dirección del ordenamiento ('ASC' o 'DESC')
  - `limite`: Número máximo de resultados por página (por defecto: 50)
  - `pagina`: Número de página para paginación (por defecto: 1)
- **Ejemplos de uso**:
  ```bash
  # Filtrar organizadores por nombre
  curl -X GET "http://localhost:3000/api/organizadores/filtrar?nombre=eventos"
  
  # Buscar organizadores con un rango de seguidores
  curl -X GET "http://localhost:3000/api/organizadores/filtrar?seguidores_min=1000&seguidores_max=5000"
  
  # Obtener organizadores ordenados por seguidores de forma descendente
  curl -X GET "http://localhost:3000/api/organizadores/filtrar?ordenar_por=seguidores&orden=DESC"
  
  # Buscar organizadores con un dominio de correo específico
  curl -X GET "http://localhost:3000/api/organizadores/filtrar?email=gmail.com"
  ```
- **Casos de uso**:
  - Implementar búsqueda avanzada de organizadores
  - Filtrar organizadores por popularidad (número de seguidores)
  - Ordenar organizadores por diferentes criterios
  - Implementar paginación para grandes conjuntos de resultados
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "organizadores": [
      {
        "id": 1,
        "nombre": "Nombre del organizador",
        "descripcion": "Descripción del organizador",
        "seguidores": 1500,
        "email": "contacto@organizador.com",
        "telefono": "+54 11 1234-5678",
        "website": "https://www.organizador.com",
        "fecha_creacion": "2025-01-15T10:30:00.000Z"
      }
    ],
    "paginacion": {
      "total": 25,
      "pagina": 1,
      "limite": 50,
      "paginas": 1
    }
  }
  ```
- **Respuestas de error**:
  - Error del servidor (500):
    ```json
    {
      "success": false,
      "message": "Error al filtrar organizadores: [descripción del error]"
    }
    ```

#### Obtener un organizador por ID
- **Endpoint**: `/api/organizadores/[id]`
- **Método**: GET
- **Descripción**: Obtiene un organizador específico por su ID.
- **Parámetros de ruta**:
  - `id`: Identificador único del organizador (número entero)
- **Ejemplo de uso**:
  ```bash
  # Obtener el organizador con ID 1
  curl -X GET "http://localhost:3000/api/organizadores/1"
  ```
- **Casos de uso**:
  - Mostrar detalles completos de un organizador específico
  - Obtener información de contacto del organizador de un evento
  - Verificar la existencia de un organizador
  - Obtener información para editar un organizador
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "organizador": {
      "id": 1,
      "nombre": "Nombre del organizador",
      "descripcion": "Descripción detallada del organizador",
      "seguidores": 1500,
      "email": "contacto@organizador.com",
      "telefono": "+54 11 1234-5678",
      "website": "https://www.organizador.com",
      "fecha_creacion": "2025-01-15T10:30:00.000Z"
    }
  }
  ```
- **Respuestas de error**:
  - Organizador no encontrado (404):
    ```json
    {
      "success": false,
      "message": "Organizador no encontrado"
    }
    ```
  - Error del servidor (500):
    ```json
    {
      "success": false,
      "message": "Error al obtener organizador: [descripción del error]"
    }
    ```
        "email": "organizador@ejemplo.com",
        "telefono": "123456789",
        "website": "https://organizador.com",
        "fecha_creacion": "2025-01-01T00:00:00.000Z"
      }
    ]
  }
  ```

### Subida de archivos

#### Subir un archivo
- **Endpoint**: `/api/upload`
- **Método**: POST
- **Descripción**: Sube un archivo al servidor (principalmente imágenes).
- **Parámetros** (FormData):
  - `file`: Archivo a subir
  - `tipo`: Tipo de archivo ('eventos', 'organizadores', etc.)
- **Respuesta exitosa**:
  ```json
  {
    "success": true,
    "url": "/uploads/eventos/imagen.jpg"
  }
  ```

## Notas importantes

- Todas las solicitudes que requieren autenticación deben incluir un encabezado `Authorization` con el formato `Bearer {token}`.
- Los errores devuelven un código de estado HTTP apropiado (400, 401, 404, 500) junto con un mensaje descriptivo.
- Las fechas se manejan en formato ISO 8601 (YYYY-MM-DDTHH:MM:SS.sssZ).
- Las imágenes subidas se almacenan en la carpeta `/public/uploads/` y son accesibles públicamente.