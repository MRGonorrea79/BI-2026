# ESCUELA POLITÉCNICA NACIONAL  
# Facultad de Ingeniería de Sistemas  
# Ingeniería de Software  
# Business Intelligence  


## Laboratorio #1  
### Instalación y uso de Pentaho Data Integration (PDI)  



**Integrantes:**  
Javier Angulo  
Jotcelyn Godoy  
Michael Tipan  
Javier Quilumba  
Cristian Robles

**Docente:**  
Diana  

**Curso:**  
Business Intelligence / GR2SW  

**Fecha:**  
21 de abril de 2026  

---

## INTRODUCCIÓN

---

## DESARROLLO
# Caso 1 - Proceso ETL en Pentaho (JSON Input → Transformación → JSON Output)

## 1. Fase de Extracción (E)
Para iniciar el proceso, se preparó la fuente de datos y la conexión inicial en Pentaho Data Integration:

- **Preparación del origen**:  
  Se creó un archivo `.json` en el directorio local con una estructura jerárquica que incluye objetos anidados (`usuario`, `encuesta`) y campos de valor simple.

- **Configuración del Input**:  
  - Se utilizó el step **JSON Input**, renombrado como *Staging*.  
  - Se vinculó la ruta del archivo local en la pestaña **File**.  
  - En la pestaña **Fields**, se definieron los objetos y campos mediante **JsonPath** para mapear la estructura del archivo a una tabla lógica de Pentaho.

---

## 2. Fase de Transformación (T)
El objetivo fue enriquecer los datos originales mediante una calificación cualitativa basada en valores numéricos.

- **Vínculo de datos**:  
  Se creó un *hop* (conector) desde el paso *Staging* hacia el step **Number Range**.

- **Lógica de Negocio (Clasificación)**:  
  - Campo de entrada: `puntuacion_satisfaccion`  
  - Campo de salida: `rango`  
  - Umbrales definidos:

    | Límite Inferior | Límite Superior | Valor Cualitativo |
    |-----------------|-----------------|-------------------|
    | 0.0             | 5.0             | Baja              |
    | 5.0             | 8.0             | Media             |
    | 8.0             | 10.0            | Alta              |

---

## 3. Fase de Carga (L)
Finalmente, los datos transformados se exportaron a un nuevo formato para su consumo posterior.

- **Configuración de Salida**:  
  Se utilizó el step **JSON Output** conectado a la salida del **Number Range**.

- **Definición de Campos**:  
  - Se especificó el directorio de destino y el nombre del archivo de salida.  
  - En la pestaña **Fields**, se seleccionaron los campos originales junto con el nuevo campo calculado `rango`.

---

## 4. Ejecución y Validación
- **Ejecución**:  
  Se corrió la transformación mediante el botón **Run**.

- **Verificación**:  
  - Se revisaron los logs de ejecución de Spoon para confirmar que no hubo errores (`I=5, O=5`).  
  - Se validó físicamente en el directorio que el archivo JSON de salida contuviera la nueva etiqueta cualitativa por cada registro procesado.

---
# Caso 2 - Proceso ETL en Pentaho (Data Grid Input → Transformación → Text file Output)

## 2. Input: Data Grid
Para el experimento de entrada, se utilizó el componente Data Grid. Este paso permite generar una tabla de datos interna sin necesidad de archivos externos.

* **Configuración:** Se definió una columna de tipo `String` denominada `Nombre`.
  
  ![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image.png>)

* **Datos ingresados:** Se registraron 5 filas con nombres propios (Cristian, Javier, Juan, Pedro y Pepito).

    ![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-1.png>)


## 2. Transformation: String Operations
Como transformación, se aplicó el componente **String Operations** para demostrar la capacidad de Pentaho en la normalización de cadenas de texto.

* **Configuración:** Se seleccionó el campo `Nombre` y se aplicó la función **Upper** para poner todas las letras en Mayúsculas.
* **Limpieza:** Se activó la opción **Trim type: both** para asegurar la eliminación de espacios en blanco innecesarios, garantizando la integridad de los datos.

![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-2.png>)
---

## 3. Output: Text File Output
Para cerrar el ciclo del proceso ETL, se utilizó un componente de salida de archivos de texto plano.

* **Función:** Exportar los datos ya transformados a un archivo físico.
* **Resultado:** El archivo final contiene los nombres procesados y estandarizados, listos para ser consumidos por otros módulos de software o sistemas de reportería.

![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-3.png>)
---

## 4. Resultados de Ejecución
Se procedió a correr la transformación localmente en Spoon. El sistema reportó una ejecución exitosa de extremo a extremo.

* **Estado:** Finalización exitosa (Checks verdes en todos los componentes).
* **Métricas:** Se procesaron correctamente las 5 filas de entrada, pasando por la transformación y llegando al archivo de salida sin pérdida de datos.

![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-4.png>)

![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-5.png>)

![alt text](<Capturas_Proceso ETL en Pentaho (Data Grid Input - Transformación - Text file Output)/image-6.png>)
---

# Caso 3 - CSV File Input → Calculator → JSON Output

**Integrante:** Jonathan Tipan

Se trabajó con un archivo de ventas en formato `.csv` para calcular el subtotal de cada producto y exportar el resultado en formato JSON.

---

## Archivo de entrada `ventas.csv`

![Archivo ventas.csv](imagenes/image.png)

---

## Pasos

### 1. CSV File Input

Se arrastró el step **CSV File Input** (carpeta *Input*) al canvas y se configuró apuntando al archivo `ventas.csv`, con delimitador `,` y la opción **Header row present** activada.

Luego se utilizó **Get Fields** para detectar automáticamente las columnas.

![Configuración del CSV File Input con los campos detectados](imagenes/image-1.png)

---

### 2. Calculator

Se agregó el step **Calculator** (carpeta *Transform*) y se conectó al anterior con `Shift + arrastre`.

Se definió un único campo calculado:

| Nuevo campo | Operación | Campo A           | Campo B    |
|-------------|-----------|-------------------|------------|
| `subtotal`  | A * B     | `precio_unitario` | `cantidad` |

De esta manera, el subtotal se obtiene multiplicando el precio unitario por la cantidad de productos.

![Configuración del Calculator](imagenes/image-2.png)

---

### 3. JSON Output

Se agregó el step **JSON Output** (carpeta *Output*) y se conectó al Calculator.

**Pestaña General:**

- Se seleccionó la operación `Write to file`
- Se configuró el nombre del archivo de salida, por ejemplo: `${Internal.Entry.Current.Directory}/resultados`
- Se dejó la extensión como `json`
- Se configuró la codificación en **UTF-8**
- En **Nr rows in a block** se colocó un valor mayor al número de registros (por ejemplo `8`) para que toda la salida se genere en un solo archivo JSON

![Pestaña General del JSON Output](imagenes/image-3.png)

**Pestaña Fields:**

- Se utilizó **Get Fields** para incluir los campos:
  - `producto`
  - `precio_unitario`
  - `cantidad`
  - `subtotal`

![Pestaña Fields del JSON Output](imagenes/image-4.png)

---

### 4. Resultado

Al ejecutar la transformación con `F9`, el proceso finalizó correctamente y se generó el archivo `resultados_0.json` con la información calculada.

La siguiente imagen muestra la ejecución exitosa de la transformación y el flujo completo con los tres steps conectados: **CSV File Input**, **Calculator** y **JSON Output**.

![Ejecución exitosa y flujo de la transformación](imagenes/image-6.png)

A continuación, se presenta el contenido generado en el archivo JSON de salida:

![Contenido del archivo JSON generado](imagenes/image-5.png)

---

# Caso 4 - Proceso ETL en Pentaho (YAML Input → Transformación JS → Table Output HSQLDB)

**Integrante:** Javier Quilumba

<div style="background-color: #f4f6f8; border-left: 4px solid #1a73e8; padding: 15px; border-radius: 4px; font-family: sans-serif;">
  <b style="color: #1a73e8;">Contexto y Propuesta:</b><br/>
  Implementación de un flujo de Extracción, Transformación y Carga (ETL) orientado a procesar remesas de catálogos nativos de integraciones API (YAML), aplicar reglas de negocio sobre métricas comerciales mediante algoritmos, y consolidar los registros resultantes en una tabla de Staging dentro de un motor relacional embebido.
</div>

<br/>

#### Arquitectura del Flujo y Nodos de Operación

A continuación, se fundamenta rigurosamente el diseño de las etapas vectoriales. El proceso abarca la lectura transaccional, el procesado Rhino nativo intermedio y la carga bajo directrices relacionales:

<table style="width: 100%; border-collapse: collapse; font-family: sans-serif; font-size: 0.95em;">
  <thead style="background-color: #1a73e8;">
    <tr>
      <th style="padding: 10px; border: 1px solid #e0e0e0; text-align: left; color: #ffffff;">Nodo PDI</th>
      <th style="padding: 10px; border: 1px solid #e0e0e0; text-align: left; color: #ffffff;">Categoría</th>
      <th style="padding: 10px; border: 1px solid #e0e0e0; text-align: left; color: #ffffff;">Justificación Estructural</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;"><b>YAML Input</b></td>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;">Entrada</td>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;">Consumo de jerarquías modernas. Alternativa de arquitectura avanzada frente a formatos limitados como CSV, facilitando la ingesta directa sin intermediarios.</td>
    </tr>
    <tr>
      <td style="padding: 10px; border: 1px solid #e0e0e0;"><b>Modified JS Value</b></td>
      <td style="padding: 10px; border: 1px solid #e0e0e0;">Transformación</td>
      <td style="padding: 10px; border: 1px solid #e0e0e0;">Consolidación de limpieza de cadenas (strings), casting de métricas numéricas y árboles de decisión (generación de KPIs financieros) mediante el Engine Rhino.</td>
    </tr>
    <tr>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;"><b>Table Output</b></td>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;">Salida</td>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;">Despliega transaccionalidad hacia HSQLDB (Hypersonic). Opera la inserción en BD bajo metodologías de Full-Reload previo para garantizar integridad base (<i>Truncate Table</i>).</td>
    </tr>
  </tbody>
</table>

<br/>

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/00_Transformation_Flow.png" alt="Transformation Flow" width="85%" style="border: 1px solid #ccc; max-width: 800px; display: block; margin: 0 auto;"/>
  <p style="color: #666; font-size: 0.9em; margin-top: 5px;"><i>Fig 1. Transformation Flow.</i></p>
</div>

<br/>

#### 1. Fase de Extracción Transaccional

El flujo inicia asimilando el set de datos `products_catalog.yaml`. Dado el esquema de parser nativo, resultó indispensable formatear el origen como **YAML Multi-Documento** (bloques seccionados mediante el separador estricto `---`) para proveer iteratividad. Al mandar a ejecutar el procesamiento de metadatos, el nodo Pentaho logró rastrear y tipificar correctamente la matriz extrayendo los 10 vectores dimensionales.

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/01_YAML_Input_Fields.png" alt="YAML Input Fields" width="85%" style="border: 1px solid #ccc; max-width: 800px; display: block; margin: 0 auto;"/>
  <p style="color: #666; font-size: 0.9em; margin-top: 5px;"><i>Fig 2. YAML Input Fields.</i></p>
</div>

<br/>

#### 2. Fase de Enriquecimiento Lógico y Profiling

Se curaron las inconsistencias de origen integrando validación cruzada y casting sobre el entorno de procesamiento Rhino JavaScript Engine asimilado en PDI. Desde el algoritmo base se derivan transaccionalmente 8 nuevas columnas artificiales de control dimensional.

```javascript
/* 1. CASTING DE DATOS (Forzado numérico nativo) */
var precio_num = parseFloat(String(unit_price).trim());
var qty_num    = parseInt(String(stock_quantity));

/* 2. CÁLCULO DE MACRO-METRICAS FINANCIERAS */
var precio                 = String(precio_num.toFixed(2));
var valor_total_inventario = String((precio_num * qty_num).toFixed(2));

/* 3. SEMÁNTICA COMERCIAL E INVENTARIO */
var clasificacion_precio = precio_num > 500 ? "PREMIUM" 
                         : precio_num >= 100 ? "MID-RANGE" : "ECONOMICO";

var estado_stock = qty_num == 0 ? "SIN STOCK" 
                 : qty_num <= 20 ? "CRITICO" 
                 : qty_num <= 50 ? "BAJO" : "NORMAL";

/* 4. NORMALIZACIÓN ALFANUMÉRICA FINAL */
var estado_activo  = String(active) == "true" ? "ACTIVO" : "INACTIVO";
var categoria_norm = String(category).trim().toUpperCase();
var nombre_norm    = String(name).trim().toUpperCase();
```

<div style="background-color: #fff4e5; border-left: 4px solid #ff9800; padding: 15px; margin-top: 15px; border-radius: 4px; font-family: sans-serif;">
  <b style="color: #ed6c02;">Consideración de Integración Java:</b> Para anular la alta propensión de falla de compatibilidad sobre el motor transpilador Rhino de PDI 10.x conocida como <code>ClassCastException</code> (al mapear el tipo Javascript <i>UniqueTag</i> frente a <code>java.lang.Number</code>), todos los retornos fueron deliberadamente preformateados y casteados mediante la directiva de contención <code>String()</code>. Como requerimiento consecuente se tipificó su salida como alfanumérica pura en los campos correspondientes.
</div>

<br/>

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/02_JavaScript_Script.png" alt="JavaScript Script" width="85%" style="border: 1px solid #ccc; max-width: 800px; display: block; margin: 0 auto;"/>
  <p style="color: #666; font-size: 0.9em; margin-top: 5px;"><i>Fig 3. JavaScript Script.</i></p>
</div>

<br/>

#### 3. Ensamblaje Estructural Secundario

Para lograr unificar el stream con mayor granularidad, el nodo medio se validó visualmente en aislamiento en una subcapa posterior (Canvas de Progreso).

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/03_Canvas_Paso_Medio.png" alt="Canvas Paso Medio" width="85%" style="border: 1px solid #ccc; max-width: 800px; display: block; margin: 0 auto;"/>
  <p style="color: #666; font-size: 0.9em; margin-top: 5px;"><i>Fig 4. Canvas Paso Medio.</i></p>
</div>

<br/>

#### 4. Fase de Transacción Persistente Relacional

Una vez estabilizada la metadata limpia en memoria, el conjunto prosigue a su persistencia física sobre la tabla parametrizada meta denominada `STG_PRODUCTOS`. Acorde a los mejores lineamientos de pruebas unitarias sobre entornos aislados (sin dependencias directas a SQL Server), se enlazó la conexión nativa JDBC apuntando hacia **Hypersonic (HSQLDB)** utilizando referenciación de URI en formato File.

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/04_Conexion_BD_Hypersonic.png" alt="Conexión BD Hypersonic" width="85%" style="border: 1px solid #ccc; max-width: 800px; display: block; margin: 0 auto;"/>
  <p style="color: #666; font-size: 0.9em; margin-top: 5px;"><i>Fig 5. Conexión BD Hypersonic.</i></p>
</div>

<br/>

Para agilizar el modelo DDL, la sentencia `CREATE TABLE` fue autogenerada de antemano garantizando el matching exacto del Buffer, declarando tipado plano equivalente acorde a los requisitos de la herramienta SQL destino.

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/05_SQL_CREATE_Table.png" alt="SQL CREATE Table" width="85%" style="border: 1px solid #ccc; max-width: 800px; display: block; margin: 0 auto;"/>
  <p style="color: #666; font-size: 0.9em; margin-top: 5px;"><i>Fig 6. SQL CREATE Table.</i></p>
</div>

<br/>

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/06_Table_Output_Configuracion.png" alt="Table Output Configuración" width="85%" style="border: 1px solid #ccc; max-width: 800px; display: block; margin: 0 auto;"/>
  <p style="color: #666; font-size: 0.9em; margin-top: 5px;"><i>Fig 7. Table Output Configuración.</i></p>
</div>

<br/>

#### 5. Ejecución Integral Analítica y Verificación de Estado

La fase de validación evalúa sincrónicamente la salud del proceso batch instanciando el evento de lanzamiento Run (F9). Las tabulaciones de logging validan y reportan la asimilación correcta sin presentarse un solo quiebre de lectura.

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/07_Step_Metrics_Resultados.png" alt="Step Metrics Resultados" width="85%" style="border: 1px solid #ccc; max-width: 800px; display: block; margin: 0 auto;"/>
  <p style="color: #666; font-size: 0.9em; margin-top: 5px;"><i>Fig 8. Step Metrics Resultados.</i></p>
</div>

El veredicto final es evaluable mediante el visor directo de matriz. En base a los componentes previsualizados en "Preview Data", detectamos la asignación algorítmica impecable de rubros jerárquicos como categorías dinámicas (`PREMIUM`, `ECONOMICO`) e inventario sensible (ej. `SIN STOCK` o `CRITICO`). Esta capa Staging consolidada permite alimentar ahora Data Warehouses, APIs de reportería o Cubos OLAP garantizando que todas las tuplas cumplen criterios formales de Data Quality empresarial.

<div align="center">
  <img src="./Recursos_ETL_YAML-SQL_Quilumba/08_Preview_Data_Enriquecida.png" alt="Preview Data Enriquecida" width="85%" style="border: 1px solid #ccc; max-width: 800px; display: block; margin: 0 auto;"/>
  <p style="color: #666; font-size: 0.9em; margin-top: 5px;"><i>Fig 9. Preview Data Enriquecida.</i></p>
</div>

<br/>

#### 6. Aprendizajes de este caso

<table style="width: 100%; border-collapse: collapse; font-family: sans-serif; font-size: 0.95em;">
  <thead style="background-color: #3f51b5;">
    <tr>
      <th style="padding: 10px; border: 1px solid #e0e0e0; text-align: left; width: 35%; color: #ffffff;">Aspecto Evaluado</th>
      <th style="padding: 10px; border: 1px solid #e0e0e0; text-align: left; color: #ffffff;">Aprendizaje Obtenido</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;"><b>Uso de Herramientas</b></td>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;">Pentaho Data Integration (Spoon) facilita la creación visual de pipelines. Se aprendió a conectar archivos estructurados (YAML) e integrar lógica de validación interna con JavaScript puro.</td>
    </tr>
    <tr>
      <td style="padding: 10px; border: 1px solid #e0e0e0;"><b>Potencial de la Tecnología</b></td>
      <td style="padding: 10px; border: 1px solid #e0e0e0;">Demuestra gran utilidad práctica para Inteligencia de Negocios, ya que es capaz de migrar, homogeneizar y automatizar cargas hacia motores relacionales en pocos pasos (ej. HSQLDB).</td>
    </tr>
    <tr>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;"><b>Ingeniería de Software</b></td>
      <td style="padding: 10px; border: 1px solid #e0e0e0; background-color: #fafbfc;">Conocer ETL amplía mi alcance técnico. Permite ir más allá de desarrollar aplicaciones y empezar a construir flujos de datos donde se limpian, refinan y preparan tablas para equipos de análisis.</td>
    </tr>
  </tbody>
</table>

---

## CONCLUSIÓN

