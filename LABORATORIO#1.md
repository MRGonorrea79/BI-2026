# ESCUELA POLITÉCNICA NACIONAL
# Facultad de Ingeniería de Sistemas
# Ingeniería de Software
# Business Intelligence

## Laboratorio #1
### Instalación y uso de Pentaho Data Integration (PDI)

**Integrantes:**
* Javier Angulo
* Jotcelyn Godoy
* Michael Tipan
* Javier Quilumba
* Cristian Robles

**Docente:**
SILVIA DIANA MARTINEZ MOSQUERA

**Curso:**
Business Intelligence / GR2SW

**Fecha:**
21 de abril de 2026

## INTRODUCCION

En el contexto de la Inteligencia de Negocios, los procesos de Extracción, Transformación y Carga (ETL) constituyen una base fundamental para la integración y preparación de datos provenientes de diversas fuentes. Estas técnicas permiten convertir datos heterogéneos en información estructurada y de calidad, lista para su análisis y toma de decisiones.

El presente laboratorio tiene como objetivo la instalación y uso de Pentaho Data Integration (PDI), una herramienta ampliamente utilizada para el diseño de procesos ETL de manera visual y eficiente. A través de distintos casos prácticos, se exploran diversas fuentes de datos como JSON, CSV, XML y YAML, aplicando transformaciones que incluyen limpieza, normalización, enriquecimiento y estructuración de la información.

## DESARROLLO

# Caso 1 - Proceso ETL (JSON Input → Number Range → JSON Output)
**Responsable:** Cristian Robles

### 1. Fase de Extracción (E)
Para iniciar el proceso, se preparó la fuente de datos y la conexión inicial en PDI:
- **Preparación del origen:** Se creó un archivo `.json` local con una estructura jerárquica de usuarios y encuestas.
- **Configuración del Input:** Se utilizó el step **JSON Input** (renombrado como *Staging*). Se mapearon los campos mediante **JsonPath** para transformar la estructura anidada en una tabla lógica de Pentaho.

### 2. Fase de Transformación (T)
El objetivo fue enriquecer los datos originales mediante una calificación cualitativa basada en valores numéricos.
- **Lógica de Negocio:** Se integró el step **Number Range** para clasificar el campo `puntuacion_satisfaccion`.
- **Umbrales definidos:**
  | Límite Inferior | Límite Superior | Valor Cualitativo |
  |-----------------|-----------------|-------------------|
  | 0.0             | 5.0             | Baja              |
  | 5.0             | 8.0             | Media             |
  | 8.0             | 10.0            | Alta              |

### 3. Fase de Carga (L)
Finalmente, los datos transformados se exportaron para su consumo posterior.
- **Configuración de Salida:** Se utilizó el step **JSON Output** conectado a la salida del step de transformación.
- **Definición de Campos:** Se especificó el directorio de destino y se seleccionaron los campos originales junto con el nuevo campo calculado `rango`.

### 4. Ejecución y Validación
- **Ejecución:** Se corrió la transformación mediante el botón **Run**.
- **Verificación:** Se revisaron los logs de ejecución de Spoon para confirmar el éxito (`I=5, O=5`) y se validó físicamente que el archivo JSON de salida contuviera la nueva etiqueta cualitativa.

---

# Caso 2 - Proceso ETL (Data Grid → String Operations → Text file Output)
**Responsables:** Michael Tipan / Jotcelyn Godoy

### 1. Fase de Extracción (E)
- **Configuración:** Se utilizó el componente **Data Grid** para generar una tabla de datos interna. Se definió la columna `Nombre` de tipo String.
  ![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image.png>)
- **Datos ingresados:** Se registraron 5 filas con nombres propios (Cristian, Javier, Juan, Pedro y Pepito).
  ![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-1.png>)

### 2. Fase de Transformación (T)
- **Operación:** Se aplicó el componente **String Operations** para la normalización de cadenas de texto.
- **Configuración:** Se seleccionó el campo `Nombre`, aplicando la función **Upper** (Mayúsculas) y **Trim type: both** para eliminar espacios en blanco innecesarios.
  ![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-2.png>)

### 3. Fase de Carga (L)
- **Función:** Exportar los datos ya transformados a un archivo físico.
- **Resultado:** Se utilizó el componente **Text File Output** para generar el archivo final con los nombres estandarizados.
  ![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-3.png>)

### 4. Ejecución y Validación
- **Estado:** Finalización exitosa verificada mediante checks verdes en todos los componentes y procesamiento correcto de las 5 filas de entrada.
  ![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-4.png>)
  ![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-5.png>)
  ![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-6.png>)

---

# Caso 3 - Proceso ETL (CSV Input → Calculator → JSON Output)
**Responsable:** Jonathan Tipan

### 1. Fase de Extracción (E)
- **Origen:** Se trabajó con un archivo de ventas en formato `.csv` (`ventas.csv`).
  ![Archivo ventas.csv](imagenes/image.png)
- **Configuración:** Se configuró el step **CSV File Input** con delimitador `,` y la opción **Header row present** activada, detectando los campos automáticamente.
  ![Configuración del CSV File Input](imagenes/image-1.png)

### 2. Fase de Transformación (T)
- **Lógica:** Se implementó el step **Calculator** para calcular el subtotal de cada producto.
- **Operación:** Se definió el campo `subtotal` mediante la multiplicación: `precio_unitario * cantidad`.
  ![Configuración del Calculator](imagenes/image-2.png)

### 3. Fase de Carga (L)
- **Configuración:** Uso del step **JSON Output** configurado como `Write to file` y codificación **UTF-8**.
- **Campos:** Se incluyeron mediante **Get Fields** las columnas originales y el nuevo cálculo de subtotal.
  ![Pestaña General](imagenes/image-3.png)
  ![Pestaña Fields](imagenes/image-4.png)

### 4. Ejecución y Validación
- **Resultado:** El proceso finalizó correctamente generando el archivo `resultados_0.json`.
  ![Ejecución exitosa](imagenes/image-6.png)
  ![Contenido JSON generado](imagenes/image-5.png)

---

# Caso 4 - Proceso ETL en Pentaho (XML → Row normaliser → CSV)
**Responsable**: Javier Angulo

## 1. Extracción (E)

Se utilizó un archivo XML obtenido del dataset de W3Schools XML Simple Dataset ([https://www.w3schools.com/xml/simple.xml](https://www.w3schools.com/xml/simple.xml)), el cual contiene nodos `<food>` con datos como nombre, precio, descripción y calorías.

* Step usado: **Get Data from XML** (*Staging*)
* Loop XPath: `/breakfast_menu/food`
* Campos extraídos: `name`, `price`, `description`, `calories`

<img width="1195" height="845" alt="image" src="https://github.com/user-attachments/assets/79f48b23-e4ae-4e4f-9699-feb5c9f07581" />

<img width="1196" height="178" alt="image" src="https://github.com/user-attachments/assets/31c0d44c-5486-4837-8276-0c2bca140314" />



## 2. Transformación (T)

Se aplicaron operaciones básicas de limpieza y estructuración:

* **String Operations (Trim)**: eliminación de espacios en campos de texto. Se aplicó la operación Trim (both) a los campos de tipo texto (name, description, price) para eliminar espacios en blanco al inicio y final de cada valor.

<img width="1300" height="201" alt="image" src="https://github.com/user-attachments/assets/675c7333-bd23-4c5d-9cc9-6a2aadc79d1b" />


* **Row normaliser**: adaptación de la estructura jerárquica a formato tabular. Este paso permitió reorganizar los datos provenientes del XML en una estructura tabular más adecuada para exportación, agrupando valores relacionados en una sola fila cuando era necesario,ya que la jerarquia comun de XML no permite su transformacion a archivos planos como CSV.

<img width="1162" height="257" alt="image" src="https://github.com/user-attachments/assets/3831a79b-06c3-42df-bf6c-407b9aec7653" />
  

## 3. Carga (L)

Los datos se exportaron a formato plano:

* Step: **Text File Output (CSV)**
* Separador: coma (`,`)
* Campos: `name`, `price`, `description`, `calories`

<img width="1083" height="560" alt="image" src="https://github.com/user-attachments/assets/3fd9ce7c-42d7-44d6-84c5-ed6109c5222c" />

<img width="1084" height="561" alt="image" src="https://github.com/user-attachments/assets/a10138e1-2f56-4584-8f15-562239a0833a" />


## 4. Validación

- Se validó que el archivo CSV generado contuviera los datos correctamente limpiados,no contuviera espacios en blanco y que estos estuvieran estructurados en formato tabular.

- Se comprobó que cada registro del XML original correspondiera a una fila en el archivo CSV final.

<img width="1223" height="565" alt="image" src="https://github.com/user-attachments/assets/4eb22855-907f-4033-8c47-48cf4f5924ef" />

---

# Caso 5 - Proceso ETL (YAML Input → Modified JS Value → Table Output)

### 1. Fase de Extracción (E)

Se implementó un flujo ETL orientado a procesar un catálogo de productos en formato YAML, proveniente de integraciones tipo API.

* **Configuración del Input:**
  Se utilizó el step **YAML Input** para la lectura del archivo `products_catalog.yaml`.

* **Preparación del origen:**
  El archivo fue estructurado como **YAML Multi-Documento** (separado por `---`) para permitir la correcta iteración de registros.

* **Resultado:**
  Pentaho logró mapear correctamente los campos del archivo, extrayendo múltiples atributos por cada producto.

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/00_Transformation_Flow.png" width="85%"/>
  <p><i>Fig 1. Transformation Flow.</i></p>
</div>

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/01_YAML_Input_Fields.png" width="85%"/>
  <p><i>Fig 2. YAML Input Fields.</i></p>
</div>

---

### 2. Fase de Transformación (T)

Se aplicaron procesos de limpieza, normalización y enriquecimiento de datos mediante scripting.

* **Step utilizado:** **Modified JavaScript Value**

* **Operaciones realizadas:**

  * Conversión de tipos numéricos (casting)
  * Cálculo de métricas:

    * `valor_total_inventario = precio * cantidad`
  * Clasificación de datos:

    * Precio: `PREMIUM`, `MID-RANGE`, `ECONOMICO`
    * Stock: `SIN STOCK`, `CRITICO`, `BAJO`, `NORMAL`
  * Normalización de texto:

    * Eliminación de espacios (trim)
    * Conversión a mayúsculas

```javascript
var precio_num = parseFloat(String(unit_price).trim());
var qty_num    = parseInt(String(stock_quantity));

var precio = String(precio_num.toFixed(2));
var valor_total_inventario = String((precio_num * qty_num).toFixed(2));

var clasificacion_precio = precio_num > 500 ? "PREMIUM" 
                         : precio_num >= 100 ? "MID-RANGE" : "ECONOMICO";

var estado_stock = qty_num == 0 ? "SIN STOCK" 
                 : qty_num <= 20 ? "CRITICO" 
                 : qty_num <= 50 ? "BAJO" : "NORMAL";

var estado_activo  = String(active) == "true" ? "ACTIVO" : "INACTIVO";
var categoria_norm = String(category).trim().toUpperCase();
var nombre_norm    = String(name).trim().toUpperCase();
```

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/02_JavaScript_Script.png" width="85%"/>
  <p><i>Fig 3. JavaScript Script.</i></p>
</div>

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/03_Canvas_Paso_Medio.png" width="85%"/>
  <p><i>Fig 4. Canvas Paso Medio.</i></p>
</div>

---

### 3. Fase de Carga (L)

Los datos procesados fueron almacenados en una base de datos relacional.

* **Step utilizado:** **Table Output**
* **Base de datos:** HSQLDB (Hypersonic)
* **Configuración:**

  * Inserción en la tabla `STG_PRODUCTOS`
  * Estrategia de recarga completa (*Truncate Table*)

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/04_Conexion_BD_Hypersonic.png" width="85%"/>
  <p><i>Fig 5. Conexión BD Hypersonic.</i></p>
</div>

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/05_SQL_CREATE_Table.png" width="85%"/>
  <p><i>Fig 6. SQL CREATE Table.</i></p>
</div>

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/06_Table_Output_Configuracion.png" width="85%"/>
  <p><i>Fig 7. Table Output Configuración.</i></p>
</div>

---

### 4. Ejecución y Validación

* **Ejecución:**
  Se ejecutó la transformación mediante **Run (F9)** en Pentaho Spoon.

* **Verificación:**

  * No se presentaron errores en la ejecución
  * Los logs confirmaron el procesamiento correcto de los datos
  * Se verificó en la vista previa que los registros contenían las nuevas métricas y clasificaciones

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/07_Step_Metrics_Resultados.png" width="85%"/>
  <p><i>Fig 8. Step Metrics Resultados.</i></p>
</div>

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/08_Preview_Data_Enriquecida.png" width="85%"/>
  <p><i>Fig 9. Preview Data Enriquecida.</i></p>
</div>

---

---

## CONCLUSIONES

En este laboratorio se evidenció la importancia de los procesos ETL en la preparación y transformación de datos dentro del ámbito de Business Intelligence, utilizando Pentaho Data Integration como herramienta principal. A través de los distintos casos, se logró trabajar con múltiples formatos de entrada como JSON, CSV, XML y YAML, aplicando técnicas de limpieza, normalización y enriquecimiento para obtener datos estructurados y consistentes,permitiendo comprender de manera práctica cómo los datos pueden ser integrados y transformados para su posterior análisis, reforzando así la relevancia de garantizar la calidad de la información como base para la toma de decisiones.
