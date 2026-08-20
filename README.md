# Ejercicio 1  -  4: Spring Boot CRUD

Este repositorio contiene el ejercicio práctico #1 de una API RESTful (CRUD) construida con Spring Boot y conectada a una base de datos MySQL.
En este ejercicio se realizaron algunas configuraciones, se solucionó un problema de validación y se documentó y probó la API utilizando Swagger UI.

## Actividades realizadas:

1. **Configuración de Base de Datos y Credenciales (MySQL):**
   - Instalación y configuración de MySQL Server y MySQL Workbench.
   - Actualización de las credenciales de conexión (`username` y `password`) en el archivo `application.properties`.


2. **Integración de Swagger (OpenAPI):**
   - Se añadió la dependencia `springdoc-openapi-starter-webmvc-ui` en el archivo `pom.xml`.
   - Esto dio chance de interactuar visualmente con los endpoints a través de `http://localhost:8080/swagger-ui/index.html`, facilitando la prueba de los métodos HTTP como se verá más abajo.

3. **NullPointerException en Validación:**
   - Durante las primeras pruebas del método `POST`, ocurrió un error 500 (`NullPointerException`).
   - **La causa:** La validación personalizada `@IsExistsDb` intentaba ejecutarse dos veces: la primera en el `ProductController` (con el servicio inyectado de forma correcta por Spring) y la segunda internamente por Hibernate al guardar la entidad (donde el servicio no se inyectaba).
   - **La solución colocada:** Para intentar arreglarlo, se incluyó una cláusula de seguridad en la clase `IsExistsDbValidation.java` (la cual es `if (service == null) return true;`) para prevenir la falla durante el ciclo de vida de persistencia de Hibernate, confiando únicamente en la validación inicial de la capa web, esto fue para hacer el cambio minimo posible y mantener el código como estaba.

## Evidencias de Pruebas (CRUD)

A continuación se documentan las pruebas de las operaciones CRUD realizadas desde Swagger UI, con sus comprobaciones en la base de datos:

### 1. Creación de Producto (POST)
Inserción correcta de un nuevo producto en la API y su confirmación en MySQL.
- ![POST a la API](/capturas/1.POST.png)
- ![Verificación en MySQL](/capturas/2.Post_en_mysql.png)

### 2. Consulta de Productos (GET)
Lectura de registros, tanto el listado general como la consulta por ID específico.
- ![Petición GET](/capturas/3.GET.png)
- ![Todos los productos](/capturas/4.GET_Todos_los_productos.png)
- ![Producto por ID](/capturas/5.GET_un_solo_producto.png)

### 3. Actualización de Producto (PUT)
Modificación de propiedades de un producto (SKU y Precio).
- ![Petición PUT](/capturas/6.PUT_Actualizacion_de_SKU_y_Price.png)
- ![Resultado PUT](/capturas/7.PUT_Resultado.png)

### 4. Eliminación de Producto (DELETE)
Eliminación del producto con el ID 3.
- ![Productos antes del DELETE](/capturas/8.Productos_antes_de_DELETE.png)
- ![Petición DELETE](/capturas/9.DELETE_de_id_3.png)
- ![Productos después del DELETE](/capturas/10.Productos_despues_de_DELETE.png)
