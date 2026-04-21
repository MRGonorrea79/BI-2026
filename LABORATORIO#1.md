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
# Proceso ETL en Pentaho (JSON Input → Transformación → JSON Output)

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

## CONCLUSIÓN
