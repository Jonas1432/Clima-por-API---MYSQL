# Clima-por-API---MYSQL

# Referencias 

En los siguientes enlaces puedes encontrar cursos en la plataforma de edu.codigoiot.com 

## Instrucciones para la creacion de la base de datos correspondiente a este ejercicio

1. Instalar MySQL Server
    - `sudo apt install mysql-server`
2. Entrar a MySQL
    - `sudo mysql`
3. Crear una nueva base de datos
    - `create databases codigoIoT;`
4. Seleccionar la base de datos creada
    - `use codigoIoT;`
5. Crear una nueva tabla que contenga los campos deseados
    - id, fecha, nombre, temperatura, humedad
    - `create table clima (id INT (6) UNSIGNED AUTO_INCREMENT PRIMARY KEY, fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP, nombre CHAR (248) NOT NULL, temperatura FLOAT (4,2), humedad INT (3));`
6. Crear un nuevo usuario para ser usado con NodeRed
    - `CREATE USER 'jonathan'@'localhost' IDENTIFIED BY '1234';`
    - `GRANT ALL PRIVILEGES ON *.* TO 'jonathan'@'localhost';`



## Notas

- Puedes consultar todas las bases de datos con el comando `show databases;`
- Puedes consultar las tablas en el interior de una base de datos selecccionada con el comando `show tables;`
- Puedes consultar la forma de la tabla con el comando `describe clima;`
- Para agregar información a la base de datos con NodeRed se requiere poner en un nodo Function la siguiente información

`msg.topic = "INSERT INTO clima (`nombre`,`temperatura`,`humedad`) VALUES ('Jonathan'," + global.get ("tempAPI")+ "," + global.get ("humAPI") + ");";`
`return msg;`

- Puedes consultar todos los datos de una tabla con el siguiente comando `SELECT * FROM clima;`

## [Resultado]

![](https://github.com/Jonas1432/Clima-por-API---MYSQL/blob/main/NodeRed-MYSQL.png)

# ---------------------------------------------------------------------------

![](https://github.com/Jonas1432/Clima-por-API---MYSQL/blob/main/ClimaAPI.png)

## Segundo paso para configurar grafana

Se llevara acabo la implementacion de grafana para la visualizacion de las graficas con valores un poco mas llamativos con el fin de tener mejor presentación.

1. Eliminar el nodo timestamp de la base de datos y añadir 4 modulos tipo template.

2. Se va modificar el siguiente query... 

`msg.topic = "INSERT INTO clima (`nombre`,`temperatura`,`humedad`) VALUES ('" + msg.payload.id + "'," + msg.payload.temp + "," + msg.payload.hum + ");";
return msg;`

3. Dirigirse a grafana en el navegador web con la dirección localhost:3000

4. Iniciar sesión el usuario y contraseña sera admin por ser primera vez, posteriormente se modificara la contraseña

5. Agregar una nueva base de datos en la opción data source con los siguientes datos:
    - Seleccionar la opcion MySQL
    - Host: localhost:3306
    - Database: Nombre de la base ya creada anteriormente
    - User & Password: nombre y contraseña que se crearon anteriormente
    - TimeZone: -05:00 para CDMX Dar clic en save&test para guardar

6. Agregar 1 panel con el parámetro temperatura y mostrara la temperaturá publica de todos, agregar un segundo panel con el parámetro humedad.

7. Para insertar los nuevos paneles de grafana se modificara `;allow_embedding = false` se modificación a lo siguiente `allow_embedding = true` y restauramos el servicion de grafana --> `sudo /bin/systemctl restart grafana-server`

8. Verificamos que funcione en el NodeRed

## [Resultados]

![](https://github.com/Jonas1432/Clima-por-API---MYSQL/blob/main/Grafana.png)

![](https://github.com/Jonas1432/Clima-por-API---MYSQL/blob/main/Grafana-clima.png)

# Créditos

* Desarrollado por Jonathan Araujo
* https://github.com/Jonas1432

