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

Siendo clases, alumnos, profesores, salas y logs como tablas que deberían existir por si mismo. En cambio inscripciones y lista_espera pasarían a ser subtablas generadas por necesidad de unión entre alumnos y clases. Estas solo deberían existir por esta relación.

Tampoco existen cambios de horario y cancelaciones ya que no lo contemplaba como una tabla aparte que debiera existir. Por lo tanto esta información se la he puesto a clase ya que si considero que la puede manejar sin problemas.

### Relaciones que he declarado

* - Un alumno puede estar en varias clases
* - Una clase puede contener varios alumnos
* - En este caso inscripciones haría la unión
* - * Alumnos N:M Clases

* - Una clase la imparte un profesor
* - Un profesor imparte varias clases (pero nunca 2 en simultaneo)
* - * Profesor 1 : n Clases

* - Una clase la imparte un profesor en una sala y en un horario
* - Una sala tiene varias clases en distintos momentos
* - Una clase se realiza en una sala unicamente
* - * Sala 1:n Clases

* - Si una clase se llena el alumno pasaría a lista de espera
* - Una clase puede tener también varios alumnos en lista de espera asi como un alumno puede estar en lista de espera de varias clases

* - Una clase puede tener varios logs o cambios (por x motivos)
* - * Clase 1 : n Logs

### Resumen de relaciones

** Profesores 1:n Clases
** Salas 1:n Clases
** Alumnos n:m Clases pero realmente:
    ** Alumnos 1:n Inscripciones
    ** Clases 1:n Inscripciones
** Alumnos n:m clases  per tambien es:
    ** Alumnos 1:n Lista_espera
    ** Clases 1:n Lista_espera
** Clases 1:n Logs

### Creacion de la BBDD
Para la creación de todas la tablas he usado: 
* - CREATE DATABASE nombre_bbdd y su uso con USE nombre_bbdd
* - CREATE TABLE nombre_tabla (
    nombre_columna TIPO CONSTRAIT
    +
    FOREING KEY (nombre de la clave foránea) REFERENCES tabla_original(id_nombre)
)
* - INSERT INTO nombre_tabla (nombre_campo, nombre_campo, nombre_campo) VALUES (valor, valor, valor)

### Mejoras
Ha esta base de datos se podrían implementar varios campos extras en alumnos, profesores, salas y logs ya que actualmente esta con los minimos para que se lea mejor.

Podría replantear una tabla horarios y cancelaciones lo que tal vez quitaria peso a clases y quedaría mejor separado.

## Modelo No Relacional

En este otro tipo de modelo he considerado 4 colecciones principales:

 1. Clases 
 2. Alumnos
 3. Profesores
 4. Salas

Siendo inscripciones, lista_espera y logs parte de la colección Clases. He decidido usar un documento embebido de clases por que es mas entendible a primera vista y no tiene tanta complejidad. Podría haberlas creado tambien por separado e incluir las _id de las colecciones a las que se refiere pero para ver la diferencia entre estructuras creo que encaja mejor.

Así mismo para la inserción de datos he incluido los 4 archivos de cada colección con un par de ejemplos en cada uno. Estos estan en formato JSON.

### Creación de la BBDD

Para generar esta BBDD no relacional he usado la interfaz de MongoDB Compass, esto permite la creación aun más rápida que en SQL.
A la hora de crear las colecciones he podido usar sus validaciones, cayendo en cuenta de que estas mismas pueden contener información extra y no siempre debe ser la misma en todas.

### Mejoras

Las mejoras que puede tener esta base de datos podria ser la creación y planificación del Schema, para validar correctamente los datos que recibimos en la misma.

Separar y no usar documentos embebidos también seria una opción en el caso de que quisiera tener una bbdd mas parecida a la relacion SQL.

Ya creada las colecciones podría tambien insertar Indices para que se pudiera mejorar el rendimiento de las consultas.


## COMPARATIVA
** ¿Qué cambia entre modelos? **
SQL:
    Es más escalable verticalmente lo que permite migrar a un servidor muchos más grande en el caso de verse limitado.
    Su estructura es relacional y formada por tablas (filas y columnas que contienen atributos).
    Es mucho más estricto, organizado y estructurado. Diría que requiere mucho más tiempo de planteamiento que NOSQL.
    Tiene sus propias reglas como atomicidad, pues sus transacciones tienen éxito o fallan por completo. O reglas de coherencia ya que este tipo de base de datos tiene que seguir validaciones exactas para evitar corrupción en los propios datos.
NOSQL:
    Es también muy escalable pero sucede de forma horizontal añadiendo más servidores o nodos.
    Su estructura es no relacional, sus colecciones no dependen de otras por lo que evita romper la propia base de datos.
    Esta orientado en columnas y su información puede variar seguna la colección.
    Tambien tiene algunas reglas. Coherencia cada solicitud recibe el resultado mas reciente o un error. Disponibilidad respecto a que cada solicitud trae un resultado sin errores. Tolerancia ya que cualquier retraso no afecta a las operaciones del sistema

** ¿Qué modelo es más flexible ? **

El modelo más flexible es el NOSQL, este permite la introducción de datos practicamente sin restricciones. Puedes hacerlo sin declarar tipos/validaciones al contrario que en SQL.

** ¿Qué es más facil de mantener? **

Considero que el mantenimiento de estas bases de datos dependerá del previo y correcto planteamiento de la estructura de la misma. Asi mismo en terminos generales diría que NOSQL, al ser más flexible evita que las bases de datos no se rompan con tanta facilidad.

** ¿Donde hay más duplicidad de datos? **
Hay mucha más duplicidad de datos en NOSQL. Esto sucede apropósito ya que de esta manera se pueden realizar consultas mucho más rápidas a la BBDD.

¿Qué ventajas tiene cada uno según el caso?
VENTAJAS SQL:
    + Estructura más clara y mejor definida
    + Consistencia e integridad de datos: debido a que usa relaciones y validaciones
    + Mas fiable y seguro: cuenta con más años de desarrollo y está más pulido
    + Consultas más avanzadas: por sus funciones y operaciones que permiten consultas complejas
    + Mejores transacciones: se pueden agrupar mas operaciones en una sola transacción.

VENTAJAS NOSQL:
    + Flexibilidad de esquema: no está atado a algo fijo
    + Buen rendimiento para cargas específicas
    + Buena gestión de BIG DATA: ideal para almacenar y gestionar gran cantidad de información
    + Nuevo: ofrecen soluciones que se adaptan bien a las tendencias actuales
