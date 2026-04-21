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

# Caso 3 — CSV File Input → Calculator → JSON Output

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

## CONCLUSIÓN
