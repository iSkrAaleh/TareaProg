# Tarea Programacion
## ¿Qué es JDBC y cuáles son sus componentes?

**JDBC** (Java Database Connectivity) es una API (Interfaz de Programación de Aplicaciones) que permite a las aplicaciones Java conectarse a bases de datos y ejecutar consultas SQL. Proporciona una interfaz estándar para interactuar con diferentes bases de datos relacionales, como MySQL, PostgreSQL, Oracle, entre otras

### Componentes principales de JDBC:

1. **Driver JDBC**: Es un componente que facilita la comunicación entre Java y el sistema de gestión de bases de datos (DBMS). Los drivers JDBC se dividen en diferentes tipos (Tipo 1, Tipo 2, Tipo 3, y Tipo 4), siendo el tipo 4 el más común en aplicaciones modernas

2. **Connection**: Esta es la interfaz que representa la conexión a la base de datos. A través de `Connection`, las aplicaciones Java pueden ejecutar consultas, actualizaciones y transacciones

3. **Statement**: Se utiliza para ejecutar sentencias SQL. Existen tres tipos de `Statement`: 
   - `Statement`: Para sentencias SQL simples
   - `PreparedStatement`: Para sentencias precompiladas
   - `CallableStatement`: Para llamar procedimientos almacenados en la base de datos

4. **ResultSet**: Es el objeto que almacena los resultados de una consulta SQL. Permite recuperar los datos devueltos por una consulta SELECT

5. **SQLException**: Es la excepción principal que maneja los errores ocurridos durante la conexión y la ejecución de sentencias SQL

---

## Librerías de Scala para conectarse a una base de datos relacional

En Scala, hay varias librerías que permiten la conexión y manipulación de bases de datos. Dos de las más comunes son **Slick** y **Doobie**

### 1. **Slick**

**Slick** es una librería de Scala que proporciona una interfaz funcional para interactuar con bases de datos. Utiliza un enfoque basado en la programación funcional y permite escribir consultas SQL directamente en el código Scala. Slick es ampliamente utilizado para aplicaciones escalables y es compatible con varios sistemas de gestión de bases de datos, incluyendo MySQL

**Características principales**:
- Permite la integración funcional con SQL
- Soporte para consultas asíncronas
- Facilita la generación de consultas SQL dinámicas
**Dependencias**:
Para usar Slick en tu proyecto, debes agregar las siguientes dependencias en tu archivo `build.sbt`:

```scala
libraryDependencies += "com.typesafe.slick" %% "slick" % "3.5.0"
libraryDependencies += "com.typesafe.slick" %% "slick-hikaricp" % "3.5.0"
```

---

### 2. **Doobie**

**Doobie** es una librería de acceso a bases de datos en Scala que sigue un enfoque más directo y explícito para interactuar con bases de datos SQL. Proporciona un enfoque funcional y es muy eficaz para realizar consultas SQL con seguridad en cuanto a tipos.

**Características principales**:
- Integración sencilla con otras librerías funcionales.
- Uso de `Transactor` para gestionar la conexión.
- Resultados seguros con la validación de tipos en tiempo de compilación.

**Dependencias**:
Para usar Doobie, agrega las siguientes dependencias en el archivo `build.sbt`:

```scala
libraryDependencies += "org.tpolecat" %% "doobie-core" % "1.0.0-M5"
libraryDependencies += "org.tpolecat" %% "doobie-hikari" % "1.0.0-M5"
```

---

### Resumen de las diferencias entre Slick y Doobie:

| Característica         | **Slick**                                   | **Doobie**                                  |
|------------------------|---------------------------------------------|---------------------------------------------|
| **Paradigma**           | Funcional y declarativo                    | Funcional y explícito                       |
| **Manejo de conexiones**| Utiliza HikariCP (gestion automática)       | Usualmente usa `Transactor` para gestión    |
| **Consultas**           | Consultas en DSL (Domain Specific Language) | Consultas SQL directas                      |
| **Compatibilidad**      | Soporte para SQL y funciones avanzadas     | Soporte para SQL puro, incluye JDBC        |
| **Manejo de tipos**     | Usa tipos personalizados en consultas      | Verificación de tipos en tiempo de compilación|
| **Complejidad**         | Más complejo pero flexible                  | Más simple y explícito                      |

---

## Cómo establecer una conexión a una base de datos relacional (MySQL) desde Scala

### Paso 1: Generar una base de datos en MySQL

1. Abre MySQL en tu terminal o cliente de MySQL.
2. Crea la base de datos con el siguiente comando:

```sql
CREATE DATABASE basededatos;
```

### Paso 2: Crear una tabla con datos de prueba

1. Asegúrate de que la base de datos correcta está seleccionada:

```sql
USE basededatos;
```

2. Crea la tabla `usuarios`:

```sql
CREATE TABLE usuarios (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100),
  correo VARCHAR(100)
);
```

3. Inserta algunos datos de prueba:

```sql
INSERT INTO usuarios (nombre, correo) VALUES
('Juan Pérez', 'juan.perez@example.com'),
('Ana Gómez', 'ana.gomez@example.com');
```

### Paso 3: Establecer la conexión a la base de datos desde Scala

Ahora, en tu código Scala, establece la conexión a la base de datos utilizando **Slick** o **Doobie**. Aquí te muestro un ejemplo utilizando **Slick**

#### Código Scala con Slick:

```scala
import slick.jdbc.MySQLProfile.api._
import slick.jdbc.HikariCP
import scala.concurrent.Await
import scala.concurrent.duration._

object ConexionSlick {
  val db = Database.forURL(
    "jdbc:mysql://localhost:3306/basededatos", 
    user = "root", 
    password = "tu_contraseña", 
    driver = "com.mysql.cj.jdbc.Driver"
  )

  def main(args: Array[String]): Unit = {
    // Consultar los datos de la tabla
    val query = sql"SELECT * FROM usuarios".as[(Int, String, String)]
    
    val result = db.run(query)
    val data = Await.result(result, 10.seconds)

    println("Datos de la tabla 'usuarios':")
    data.foreach(println)

    db.close()
  }
}
```

#### Explicación:
- `Database.forURL` establece la conexión utilizando JDBC, con la URL de la base de datos, nombre de usuario y contraseña
- La consulta SQL obtiene todos los datos de la tabla `usuarios`
- `Await.result` se usa para esperar los resultados de la consulta, dado que Slick trabaja de manera asíncrona
- Se imprime el resultado en la consola

### Paso 4: (Opcional) Realizar la consulta desde Scala
SQL TABLA
![image](https://github.com/user-attachments/assets/69f484de-3bd3-41ab-85aa-cd13a6620af0)

SCALA CONEXION
![image](https://github.com/user-attachments/assets/214fa2ae-c2de-42ba-a15e-196f3e33e444)



---
