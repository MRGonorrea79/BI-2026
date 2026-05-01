# ESCUELA POLITÉCNICA NACIONAL
# Facultad de Ingeniería de Sistemas
# Ingeniería de Software
# Business Intelligence

## Laboratorio #2
### Implementación del proceso ETL en Pentaho

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
1 de Mayo de 2026

---

## Objetivos

Al finalizar esta práctica, se espera que el estudiante sea capaz de:

1. Realizar una carga inicial (Initial Load) como parte del proceso ETL.
2. Importar y cargar datos desde una base de datos PostgreSQL al entorno de trabajo.

---

## Desarrollo de la práctica

En la presente práctica de laboratorio se desarrolló un proceso ETL utilizando Pentaho Data Integration (PDI) con el objetivo de integrar, limpiar y estandarizar la información proveniente de 
diferentes sucursales de la empresa “Ferretería El Tornillo Feliz”. La conexión se realizó hacia la base de datos **datawarehouse** en el puerto 5432, específicamente en el esquema **staging**, donde se encontraban tanto la tabla de origen como la tabla de destino.

Primero utilizamos PostgreSQL para cargar la tabla inicial
<img width="1497" height="894" alt="image" src="https://github.com/user-attachments/assets/4e593cbd-6d17-4c86-8dd4-90bae8e4726d" />


En la primera fase, se utilizó el paso **Table Input** para extraer los datos desde la tabla `staging.productos_ferreteria_raw`. 


<img width="884" height="957" alt="image" src="https://github.com/user-attachments/assets/d4c615ee-234a-43b9-ae55-216de561f57c" />

<img width="694" height="692" alt="image" src="https://github.com/user-attachments/assets/314e2ce4-317c-4e76-8f0a-06fc5e5788ec" />

Esta tabla contenía múltiples inconsistencias en los datos, tales como diferencias en la escritura de categorías, variaciones semánticas en las unidades de medida y carácteres especiales en los precios.


### Para garantizar la integridad y calidad de la información, se ejecutó una serie de pasos de transformación lógica:

Normalización de tipos de datos

Mediante el paso **Replace in String**, se eliminó caracteres innecesarios, símbolo `$` presente en el campo precio_unitario, facilitando su posterior tratamiento como valor numérico.


<img width="1233" height="315" alt="image" src="https://github.com/user-attachments/assets/5a1faa50-78d0-4816-98d4-593439f983b0" />

Limpieza de sintaxis

Se utilizó el paso **String Operations** para la eliminación de espacios en blanco excedentes (trim) y transformaciones generales sobre los datos. 


<img width="1313" height="755" alt="image" src="https://github.com/user-attachments/assets/f011f33a-088b-4fc7-9077-c28c1bd82b2f" />


Estandarización de categorías

El paso **Value Mapper** permitió la estandarización de las categorías. A través de este componente, se mapearon diferentes representaciones de una misma categoría (por ejemplo: “Htas.”, “herram.”, “HERRAMIENTAS”) hacia un único valor uniforme como “Herramientas”, asegurando consistencia en la información.


<img width="954" height="993" alt="image" src="https://github.com/user-attachments/assets/dc388295-fb9c-4b7a-adc2-b592fbb0437d" />

Normalización de unidades (Expresiones Regulares)

Se implementó un paso avanzado de Replace in String utilizando expresiones regulares **(Regex)** mediante RexGen para la limpieza y estandarización de las unidades de medida. Esto permitió identificar patrones similares (como “1 und”, “1 unidad”, “1u”, “1 Lt”, “1L”) y transformarlos en un formato homogéneo, garantizando uniformidad en los datos.

<img width="1358" height="402" alt="image" src="https://github.com/user-attachments/assets/9740004d-2958-4798-a351-caf958c0bd3d" />

Normalización Final

Se aplicó un segundo paso de **String Operations** para asegurar que todos los campos cumplieran con el formato requerido antes de su carga.

<img width="1310" height="382" alt="image" src="https://github.com/user-attachments/assets/cb8037e4-569b-46dd-b8aa-907ddb0207d8" />


Finalmente, se utilizó el paso **Table Output** para cargar los datos procesados en la tabla `staging.productos_ferreteria_clean`. Como resultado, se obtuvo un conjunto de datos limpio, estructurado y estandarizado, listo para su uso en procesos analíticos dentro del Data Warehouse.

<img width="840" height="288" alt="image" src="https://github.com/user-attachments/assets/da3c1f95-80df-4f35-b38f-7c60169879ee" />

Tabla resultante visualizada en PostgreSQL
<img width="1263" height="729" alt="image" src="https://github.com/user-attachments/assets/cfec3939-d12b-4451-b4c4-3d18aefb5707" />

Este proceso permitió cumplir con los requerimientos planteados por el cliente, demostrando el uso eficiente de herramientas ETL y técnicas de limpieza de datos para mejorar la calidad de la información.


