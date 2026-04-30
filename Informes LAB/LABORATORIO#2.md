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
diferentes sucursales de la empresa “Ferretería El Tornillo Feliz”. La conexión se realizó hacia la base de datos **datawarehouse**, específicamente en el esquema **staging**, donde se encontraban tanto la tabla de origen como la tabla de destino.



En la fase inicial, se utilizó el paso **Table Input** para extraer los datos desde la tabla `staging.productos_ferreteria_raw`. Esta tabla contenía múltiples inconsistencias en los datos, tales como diferencias en la escritura de categorías, variaciones en las unidades de medida y formatos incorrectos en los precios.

Posteriormente, se aplicó el paso **Replace in String**, el cual permitió eliminar caracteres innecesarios dentro de los datos. En este caso, se utilizó para remover el símbolo `$` presente en el campo `precio_unitario`, facilitando su posterior tratamiento como valor numérico.

A continuación, mediante el paso **String Operations**, se realizaron transformaciones generales sobre los datos, tales como la normalización de texto (uso de mayúsculas o minúsculas) y la eliminación de espacios en blanco innecesarios. Estas operaciones ayudaron a mejorar la consistencia de los campos textuales.

En la siguiente etapa, se utilizó el paso **Value Mapper**, el cual fue fundamental para la estandarización de las categorías. A través de este componente, se mapearon diferentes representaciones de una misma categoría (por ejemplo: “Htas.”, “herram.”, “HERRAMIENTAS”) hacia un único valor uniforme como “Herramientas”, asegurando consistencia en la información.

En el antepenúltimo paso, se empleó nuevamente **Replace in String**, esta vez haciendo uso de **expresiones regulares (Regex) mediante RexGen** para la limpieza y estandarización de las unidades de medida. Esto permitió identificar patrones similares (como “1 und”, “1 unidad”, “1u”, “1 Lt”, “1L”) y transformarlos en un formato homogéneo, garantizando uniformidad en los datos.

Posteriormente, se aplicó un segundo paso de **String Operations** para reforzar la normalización final de los datos, asegurando que todos los campos cumplieran con el formato requerido antes de su carga.

Finalmente, se utilizó el paso **Table Output** para cargar los datos procesados en la tabla `staging.productos_ferreteria_clean`. Como resultado, se obtuvo un conjunto de datos limpio, estructurado y estandarizado, listo para su uso en procesos analíticos dentro del Data Warehouse.

Este proceso permitió cumplir con los requerimientos planteados por el cliente, demostrando el uso eficiente de herramientas ETL y técnicas de limpieza de datos para mejorar la calidad de la información.
