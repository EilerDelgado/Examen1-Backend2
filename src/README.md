# Examen Backend 2 – Correcciones de Modelos y Conexión a BD

## Descripción
Este proyecto es una aplicación Spring Boot diseñada para depurar y estructurar la capa de modelos (entidades JPA) y establecer una conexión con una base de datos relacional local llamada `develop_db`. Este README documenta los cambios realizados en las entidades y la configuración aplicada para la conexión a la base de datos.

## Requisitos
- **JDK**: 17 o superior
- **Maven**: 3.8 o superior
- **XAMPP**: MySQL/MariaDB activo
- **IDE**: IntelliJ IDEA (recomendado)
- **Base de datos**: MySQL con la base de datos `develop_db` creada

## Cambios en las Entidades

### Entidad `Usuario`
- Corregida la anotación `@Entity` (antes escrita incorrectamente como `@Entit`).
- Ajustadas las anotaciones de persistencia:
    - `@GeneratedValue(strategy = GenerationType.IDENTITY)` para el campo `id` (antes incompleta).
    - `@Column(name = "id_usuario")` y `@Column(name = "correo_electronico", unique = true)` (antes mal escritas como `@Colun`).
- Creado e importado el **enum `TipoUsuario`** con valores: `ADMIN`, `CLIENTE`, `USUARIO`.
- Añadidos **getters y setters** para todos los atributos: `id`, `nombre`, `correoElectronico`, `contraseña`, `telefono`, `tipoUsuario`, `docente`.

### Entidad `Curso`
- Corregidas las anotaciones `@Entity` y `@Table(name = "cursos")`.
- Ajustadas las anotaciones de persistencia:
    - `@Id` y `@GeneratedValue(strategy = GenerationType.IDENTITY)` (antes incompletos).
    - `@Column(nullable = false, length = 100)` para el campo `nombre`.
- Corregida la relación con la entidad `Docente`:
    - Añadida `@ManyToOne` con `@JoinColumn(name = "fk_docente", referencedColumnName = "id")`.
    - Agregada `@JsonBackReference(value = "docente-curso")` para manejar la relación bidireccional.
- Corregido el **constructor** y añadido el atributo `docente`.
- Implementados **getters y setters** para todos los atributos: `id`, `nombre`, `docente`.

### Entidad `Docente`
- Corregida la anotación `@Entity` (antes escrita como `@Entit`).
- Añadido un **constructor vacío** (requerido por JPA).
- Creado un **constructor con todos los campos**: `id`, `especialidad`, `cursos`, `usuario`.
- Implementados los **getters y setters** para las listas de `cursos` y el atributo `usuario`.

## Configuración de la Base de Datos (MySQL con XAMPP)

### Crear la Base de Datos
1. Inicia **MySQL** en XAMPP.
2. Accede a **phpMyAdmin**: [http://localhost/phpmyadmin](http://localhost/phpmyadmin).
3. Crea la base de datos ejecutando el siguiente comando SQL:
   ```sql
   CREATE DATABASE develop_db;
   ```

### Configurar Spring Boot
El archivo de configuración se encuentra en `src/main/resources/application.properties`. Contenido:

```properties
spring.application.name=Examen1Back2
spring.datasource.url=jdbc:mysql://localhost:3306/develop_db
spring.datasource.username=root
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
```

**Explicación de las propiedades**:
- `spring.datasource.url`: Apunta a la base de datos `develop_db` en `localhost:3306`.
- `spring.datasource.username` / `spring.datasource.password`: Credenciales de MySQL (por defecto, `root` sin contraseña en XAMPP).
- `spring.jpa.hibernate.ddl-auto=update`: Crea o actualiza las tablas según las entidades JPA.
- `spring.jpa.show-sql=true`: Muestra las consultas SQL en la consola (útil para depuración).
- `spring.jpa.database-platform`: Especifica el dialecto de MySQL 8.

### Verificación
1. Ejecuta la aplicación con:
   ```bash
   mvn spring-boot:run
   ```
2. Revisa la consola para verificar las sentencias de Hibernate (ejemplo: `create table ...`).
3. Confirma en **phpMyAdmin** que las tablas se han creado en la base de datos `develop_db`.

## Cómo Ejecutar el Proyecto
1. Clona el repositorio:
   ```bash
   git clone https://github.com/EilerDelgado/Examen1-Backend2
   ```
2. Navega al directorio del proyecto:
   ```bash
   cd Examen1-Backend2
   ```
3. Ejecuta la aplicación:
   ```bash
   mvn spring-boot:run
   ```
4. Si hay controladores expuestos, accede a [http://localhost:8080](http://localhost:8080) o revisa los logs en la consola para confirmar la conexión.

## Recomendaciones y Buenas Prácticas
- Asegúrate de incluir **constructores** (vacío y con campos) y **getters/setters** en todas las entidades.
- Verifica siempre las anotaciones JPA (`@Entity`, `@Id`, `@GeneratedValue`, `@Column`, `@Table`).
- Usa nombres de tablas y columnas consistentes, en minúsculas y en plural cuando corresponda (ej.: `usuarios`, `cursos`, `docentes`).
- Realiza **commits pequeños y descriptivos** con prefijos como `fix:`, `refactor:`, o `config:`.
- Prueba la aplicación tras cada cambio importante (compila, ejecuta y verifica la creación de tablas).

## Historial de Commits
1. **Correcciones en `Usuario`**: Creación del enum `TipoUsuario` y ajustes en `Docente`.
2. **Correcciones en `Curso`**: Ajustes completos en la entidad.
3. **Ajustes en `Docente`**: Implementación de constructores y getters/setters.
4. **Configuración de la BD**: Configuración en `application.properties`.

## Entorno de Ejecución
- **JDK**: 17+
- **Maven**: 3.8+
- **XAMPP**: MySQL/MariaDB activo
- **IDE**: IntelliJ IDEA (recomendado)