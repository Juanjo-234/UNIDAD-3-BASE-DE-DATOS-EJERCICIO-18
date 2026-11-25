# UNIDAD-3-BASE-DE-DATOS-EJERCICIO-18
Este diagrama entidad-relación (modelo relacional) representa la estructura lógica de un **sistema de gestión de fabricación, montaje industrial y control de calidad**, diseñado para administrar el catálogo de productos, componentes, jerarquías de ensamblaje, órdenes de producción, líneas de montaje, personal y auditorías de calidad.

A continuación se detalla qué representa cada tabla y cómo se estructuran:

* **`PRODUCTO`**: Almacena el catálogo de productos terminados que fabrica la empresa.
* *Campos:* `CODIGO_PRODUCTO` (Clave Primaria), `NOMBRE_COMERCIAL`, `DESCRIPCION_TECNICA` y `TIEMPO_FABRICACION`.


* **`COMPONENTE`**: Contiene las piezas o materias primas necesarias para conformar los productos.
* *Campos:* `CODIGO_COMPONENTE` (Clave Primaria), `NOMBRE_COMERCIAL`, `LABORATORIO`, `NUM_LOTE` y `CODIGO_PRODUCTO` (Clave Foránea).


* **`PARTE`**: Tabla intermedia autorreferencial que modela estructuras jerárquicas o relaciones de ensamblaje complejo entre componentes (piezas padre e hijo).
* *Campos:* `CODIGO_COMPONENTE_PADRE` y `CODIGO_COMPONENTE` (ambos forman la clave primaria compuesta y actúan como claves foráneas).


* **`LINEA_MONTAJE`**: Gestiona las estaciones o líneas operativas de ensamblaje en planta.
* *Campos:* `CODIGO_LINEA` (Clave Primaria), `DESCRIPCION` y `ID_EMPLEADO` (Clave Foránea del responsable).


* **`ORDEN_FABRICACION`**: Registra los lotes, cantidades programadas, estados y fechas de emisión de la producción.
* *Campos:* `NUMERO_ORDEN` (Clave Primaria), `FECHA_EMISION`, `CANTIAD`, `ESTADO`, `LINEA_ASIGNADA` y `CODIGO_PRODUCTO` (Claves Foráneas).


* **`UNIDAD_PRODUCCION`**: Almacena el registro detallado de los artículos físicos fabricados bajo una orden específica.
* *Campos:* `ID_UNIDAD_PRODUCCION` (Clave Primaria), `CANTIDAD_PRODUCIDA`, `FECHA_PRODUCCION`, `NUMERO_ORDEN` y `CODIGO_PRODUCTO` (Claves Foráneas).


* **`EMPLEADO`**: Contiene la información del personal de la organización (operarios, inspectores, supervisores).
* *Campos:* `ID_EMPLEADO` (Clave Primaria), `NOMBRE`, `APELLIDO`, `EMAIL`, `ROL` y `TELEFONO`.


* **`INSPECCION_CALIDAD`**: Registra los resultados, fechas y horarios de las auditorías técnicas realizadas sobre las unidades fabricadas por parte del personal inspector.
* *Campos:* `ID_INSPECCION` (Clave Primaria), `FECHA`, `HORA`, `RESULTADO`, `ID_EMPLEADO` e `ID_UNIDAD_PRODUCCION` (Claves Foráneas).

<img width="1407" height="1072" alt="EJERCICIO 18-MR drawio" src="https://github.com/user-attachments/assets/5c38c42a-7cc6-4c31-a82c-cc026fe62ba8" />


Este diagrama muestra el **proceso completo de normalización de bases de datos** aplicado a un sistema de gestión para la fabricación, montaje y control de calidad de productos o componentes industriales, transformando una estructura inicial desnormalizada (Forma Normal 0) hasta llegar al diseño relacional optimizado en Tercera Forma Normal (3FN).

A continuación se detalla cada una de las fases representadas:

### 1. Fase Normal 0 (FN0) — Tabla No Normalizada

* Se parte de una **única tabla masiva y plana** que concentra toda la información mezclada en una sola estructura: datos de los productos, sus características técnicas, y múltiples grupos repetitivos con colecciones de datos anidados dentro de las celdas (como la lista de componentes necesarios, la lista de órdenes de fabricación, y la lista de inspecciones de calidad).
* **Problema:** Contiene grupos multivaluados y datos anidados en las celdas, lo que genera una alta redundancia de información, repetición masiva de registros y graves dificultades para realizar operaciones de actualización, mantenimiento o consultas eficientes.

---

### 2. Primera Forma Normal (1FN)

* Se eliminan los grupos repetitivos y se asegura que **todos los atributos sean atómicos** (cada celda contiene un único valor), expandiendo los registros para que cada fila represente una combinación individual y libre de valores anidados.
* Se obtiene una tabla tabular plana (`TABLA PLANTILLA`) donde cada campo es indivisible, definiéndose claves primarias compuestas, aunque todavía persiste una alta duplicidad de datos en la información de los productos, los componentes y las órdenes asociadas.

---

### 3. Segunda Forma Normal (2FN)

* Se parte de la 1FN y se eliminan las **dependencias parciales**. Se separan los atributos basándose en sus claves principales correspondientes, desglosando la información en múltiples tablas independientes y normalizadas:
* **`TABLA PRODUCTO`**: Almacena el catálogo de productos terminados y sus especificaciones.
* **`TABLA COMPONENTE`**: Contiene las piezas o elementos que conforman las estructuras.
* **`TABLA COMPORNETE_COMPONENTE_PADRE`**: Tabla intermedia para gestionar estructuras jerárquicas o ensamblajes entre componentes.
* **`TABLA ORDEN_FABRICACION`**: Registra los lotes, cantidades y estados de producción.
* **`TABLA LINEA_MONTAJE`**: Gestiona las líneas operativas de ensamblaje.
* **`TABLA UNIDAD_PRODUCCION`**: Relaciona las cantidades producidas por fecha, orden y producto.



---

### 4. Tercera Forma Normal (3FN)

* Se eliminan las **dependencias transitivas**, garantizando que ningún atributo no clave dependa de otro atributo no clave. El diseño se consolida en un modelo relacional robusto y totalmente normalizado, interconectado mediante claves foráneas (FK), lo que asegura la integridad referencial y evita anomalías para el control industrial:
* **`TABLA PRODUCTO`**, **`TABLA COMPONENTE`**, **`TABLA PARTE`** y **`TABLA ORDEN_FABRICACION`**: Consolidan la estructura principal de producción y materiales.
* **`TABLA LINEA_MONTAJE`**: Administra las estaciones de trabajo vinculadas a los empleados responsables.
* **`TABLA UNIDAD_PRODUCCION`**: Registra la trazabilidad de los artículos fabricados.
* **`TABLA EMPLEADO`**: Incorporada para centralizar los datos del personal (legajo, cargo, nombre, correo, teléfono), evitando redundancias en las líneas y controles.
* **`TABLA INSPECCION_CALIDAD`**: Registra de forma independiente los resultados, fechas y estados de las auditorías de calidad asociadas a los empleados inspectores y unidades de producción correspondientes.
<img width="6312" height="2722" alt="EJERCICIO 18-FASES DE NORMALIZACION drawio" src="https://github.com/user-attachments/assets/42f097fd-c05a-44bc-9548-a3e3060012fe" />

