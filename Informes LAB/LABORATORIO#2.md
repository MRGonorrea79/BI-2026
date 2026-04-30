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




En la fase inicial, se utilizó el paso **Table Input** para extraer los datos desde la tabla `staging.productos_ferreteria_raw`. 


<img width="884" height="957" alt="image" src="https://github.com/user-attachments/assets/d4c615ee-234a-43b9-ae55-216de561f57c" />

<img width="694" height="692" alt="image" src="https://github.com/user-attachments/assets/314e2ce4-317c-4e76-8f0a-06fc5e5788ec" />


Esta tabla contenía múltiples inconsistencias en los datos, tales como diferencias en la escritura de categorías, variaciones en las unidades de medida y formatos incorrectos en los precios.

Posteriormente, se aplicó el paso **Replace in String**, el cual permitió eliminar caracteres innecesarios dentro de los datos. En este caso, se utilizó para remover el símbolo `$` presente en el campo precio_unitario, facilitando su posterior tratamiento como valor numérico.


<img width="1233" height="315" alt="image" src="https://github.com/user-attachments/assets/5a1faa50-78d0-4816-98d4-593439f983b0" />


A continuación, mediante el paso **String Operations**, se realizaron transformaciones generales sobre los datos, tales como la eliminación de espacios en blanco innecesarios. 


<img width="1313" height="755" alt="image" src="https://github.com/user-attachments/assets/f011f33a-088b-4fc7-9077-c28c1bd82b2f" />



En la siguiente etapa, se utilizó el paso **Value Mapper**, el cual fue fundamental para la estandarización de las categorías. A través de este componente, se mapearon diferentes representaciones de una misma categoría (por ejemplo: “Htas.”, “herram.”, “HERRAMIENTAS”) hacia un único valor uniforme como “Herramientas”, asegurando consistencia en la información.


<img width="954" height="993" alt="image" src="https://github.com/user-attachments/assets/dc388295-fb9c-4b7a-adc2-b592fbb0437d" />


En el antepenúltimo paso, se empleó nuevamente **Replace in String**, esta vez haciendo uso de **expresiones regulares (Regex) mediante RexGen** para la limpieza y estandarización de las unidades de medida. Esto permitió identificar patrones similares (como “1 und”, “1 unidad”, “1u”, “1 Lt”, “1L”) y transformarlos en un formato homogéneo, garantizando uniformidad en los datos.

<img width="1358" height="402" alt="image" src="https://github.com/user-attachments/assets/9740004d-2958-4798-a351-caf958c0bd3d" />


Posteriormente, se aplicó un segundo paso de **String Operations** para reforzar la normalización final de los datos, asegurando que todos los campos cumplieran con el formato requerido antes de su carga.

<img width="1310" height="382" alt="image" src="https://github.com/user-attachments/assets/cb8037e4-569b-46dd-b8aa-907ddb0207d8" />


Finalmente, se utilizó el paso **Table Output** para cargar los datos procesados en la tabla `staging.productos_ferreteria_clean`. Como resultado, se obtuvo un conjunto de datos limpio, estructurado y estandarizado, listo para su uso en procesos analíticos dentro del Data Warehouse.

<img width="840" height="288" alt="image" src="https://github.com/user-attachments/assets/da3c1f95-80df-4f35-b38f-7c60169879ee" />


Este proceso permitió cumplir con los requerimientos planteados por el cliente, demostrando el uso eficiente de herramientas ETL y técnicas de limpieza de datos para mejorar la calidad de la información.
