# Bloque 1 - Sistema de reservas para clases y salas
## Modelo Relacional
En este tipo de modelo se han declarado  7 tablas
1. Clases
2. Alumnos
3. Profesores
4. Salas
5. Inscripciones
6. Lista_espera
7. logs

Clases, alumnos, profesores y salas son tablas que deberían existir por si solas, no obstante incripciones y lista de espera son tablas que nacen de una relación n:m entre alumnos y clases. En mi caso he dedicido prescindir de una tabla de horio y cancelaciones ya que considero que clases puede manejar esa información perfectamente sin necesidad de crear otra nueva subtabla.

### Relaciones que he declarado

 - Un alumno puede estar en varias clases
 - Una clase puede contener varios alumnos
 - En este caso inscripciones haría la unión
 - * Alumnos N:M Clases

 - Una clase la imparte un profesor
 - Un profesor imparte varias clases (pero nunca 2 en simultaneo)
 - * Profesor 1 : n Clases

 - Una clase la imparte un profesor en una sala y en un horario
 - Una sala tiene varias clases en distintos momentos
 - Una clase se realiza en una sala unicamente
 - * Sala 1:n Clases

 - Si una clase se llena el alumno pasaría a lista de espera
 - Una clase puede tener también varios alumnos en lista de espera asi como un alumno puede estar en lista de espera de varias clases

 - Una clase puede tener varios logs o cambios (por varios motivos)
 - * Clase 1 : n Logs

### Resumen de relaciones

- Profesores 1:n Clases
- Salas 1:n Clases
- Alumnos n:m Clases pero realmente:
    ** Alumnos 1:n Inscripciones
    ** Clases 1:n Inscripciones
- Alumnos n:m clases  per tambien es:
    ** Alumnos 1:n Lista_espera
    ** Clases 1:n Lista_espera
- Clases 1:n Logs

### Creacion de la BBDD
Para la creación de todas la tablas he usado: 
* CREATE DATABASE nombre_bbdd y su uso con USE nombre_bbdd
* CREATE TABLE nombre_tabla (
    nombre_columna TIPO CONSTRAIT
    +
    FOREING KEY (nombre de la clave foránea) REFERENCES tabla_original(id_nombre)
)
- INSERT INTO nombre_tabla (nombre_campo, nombre_campo, nombre_campo) VALUES (valor, valor, valor)

### Mejoras
Ha esta base de datos se podrían implementar varios campos extras en alumnos, profesores, salas y logs ya que actualmente esta con los minimos para que se lea mejor.

Podría replantear una tabla horarios y cancelaciones lo que tal vez quitaria peso a clases y quedaría mejor separado.

## Modelo No Relacional

En este otro tipo de modelo he considerado 4 colecciones principales:

 1. Clases 
 2. Alumnos
 3. Profesores
 4. Salas

Siendo Clases la colección más importante ya que esta contiene aparte de sus atributos principales (nombre_clase. fecha, inicio, hora_fin, aforo_max, activo) otros atributos embebidos:
1. profesor: id y nombre.
2. sala: id y nombre.
3. incripciones: id_alumno, nombre, activo, fecha_inscripción.
4. lista_espera: id_alumno, nombre, posicion, estado, fecha.
5. logs: id_log, accion y fecha.

De esta manera con la colección de clases se puede realizar consultas más avanzadas usando información que ya tiene por si solo sin tener otras colecciones directamente relacionadas.

### Creación de la BBDD

Para generar esta BBDD no relacional he usado la interfaz de MongoDB Compass, esto permite la creación aun más rápida que en SQL.
Así mimso para insertar los datos he usado documentos .json.
A la hora de crear las colecciones he podido usar sus validaciones, cayendo en cuenta de que estas mismas pueden contener información extra y no siempre debe ser la misma en todas.

### Mejoras

Las mejoras que puede tener esta base de datos podria ser la creación y planificación del Schema, para validar correctamente los datos que recibimos en la misma.

Separar y no usar documentos embebidos también seria una opción en el caso de que quisiera tener una bbdd mas parecida a la relacion SQL, pero le quitaría valor a usar NoSQL.

Ya creada las colecciones podría también insertar Indices para que se pudiera mejorar el rendimiento de las consultas.


## COMPARATIVA
* ¿Qué cambia entre modelos?
- SQL:
    Su estructura es relacional y hace que todas las tablas guarden relacion entre ellas mediante claves foraneas y claves primarias de aquellas tablas a las que refiere, dependiendo así las unas de las otras.
    Es mucho más estricto pues al crear la bbdd se debe validar el tipo de dato que se recibe, de lo contrario se puede llegar a corromper la bbdd entera. Es más organizado y estructurado. Diría que requiere mucho más tiempo de planteamiento que NOSQL.
- NOSQL:
    Su estructura es no relacional, por lo cual ninguna de las colecciones existen estan sujetas a otras, esto permite mayor flexibilidad a la hora de insertar, eliminar, editar o borrar datos de la propia base, pues al no guardar relación con el resto de colecciones la bbdd no se rompe.
    Esta orientado en columnas y su información puede variar según la colección teniendo asi colecciones que guardan información predeterminada y otras con más datos.

* ¿Qué modelo es más flexible ?

El modelo más flexible es el NOSQL, este permite la introducción de datos practicamente sin restricciones. Puedes hacerlo sin declarar tipos/validaciones y relaciones al contrario que en SQL.

- ¿Qué es más fácil de mantener?

Considero que el mantenimiento de estas bases de datos dependerá del previo y correcto planteamiento de la estructura de la misma. Asi mismo en terminos generales diría que NOSQL, al ser más flexible evita que las bases de datos no se rompan con tanta facilidad.

- ¿Donde hay más duplicidad de datos? 
Hay mucha más duplicidad de datos en NOSQL. Esto sucede apropósito ya que de esta manera se pueden realizar consultas mucho más rápidas a la BBDD.

- ¿Qué ventajas tiene cada uno según el caso?
* VENTAJAS SQL:
    + Estructura más clara y mejor definida
    + Consistencia e integridad de datos: debido a que usa relaciones y validaciones
    + Mas fiable y seguro: cuenta con más años de desarrollo y está más pulido
    + Consultas más avanzadas: por sus funciones y operaciones que permiten consultas complejas
    + Mejores transacciones: se pueden agrupar mas operaciones en una sola transacción.

* VENTAJAS NOSQL:
    + Flexibilidad de esquema: no está atado a algo fijo
    + Buen rendimiento para cargas específicas
    + Buena gestión de BIG DATA: ideal para almacenar y gestionar gran cantidad de información
    + Nuevo: ofrecen soluciones que se adaptan bien a las tendencias actuales
