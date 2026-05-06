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
15 de Mayo de 2026

---

## Objetivos

Al finalizar esta práctica, se espera que el estudiante sea capaz de:

1. Normalizar datos en un esquema estrella
2. Realizar la implementación del modelo físico estrella PostgreSQL.

---

## Desarrollo de la práctica

1. Introducción
Este informe detalla el proceso de normalización de datos en un esquema estrella y la implementación de consultas SQL para el análisis de ventas. Se trabajó con archivos de productos y ventas desnormalizadas para estructurar un modelo eficiente de BI.

## 2. Diagramas del Modelo Estrella (Power Pivot)

### 2.1. Modelo Estrella: Catálogo de Productos
Este diagrama representa la estructura de las tablas `fact_products`, `dim_category` y `dim_subcategory`.

![Diagrama Productos](diagramas/Diagrama%20Modelo%20Estrella_Catálogo%20de%20Productos.png)

### 2.2. Modelo Estrella: Ventas (Tabla Desnormalizada)
Normalización de los datos del archivo `Tabla_Desnormalizada_Ventas.csv` en un esquema estrella para facilitar el análisis transaccional.

![Diagrama Ventas](diagramas/Diagrama%20Modelo%20Estrella_Ventas.png)
---

## 3. Resolución de Preguntas SQL
A continuación, se presentan las consultas SQL solicitadas para el análisis de los datos:

### 1. ¿Cuántas ventas se realizaron por categoría de producto y mes?

```sql
SELECT 
    p.category,
    d.month,
    COUNT(*) AS total_ventas
FROM fact_sales f
JOIN dim_product p ON f.product_id = p.product_id
JOIN dim_date d ON f.date_id = d.date_id
GROUP BY p.category, d.month
ORDER BY p.category, d.month;
```

### 2. ¿Cuál es el ingreso total (ventas) por cliente y género?

```sql
SELECT 
    c.customerkey,
    c.gender,
    SUM(f.salesamount) AS total_ingreso
FROM fact_sales f
JOIN dim_customer c ON f.customer_id = c.customer_id
GROUP BY c.customerkey, c.gender
ORDER BY total_ingreso DESC;
```

### 3. ¿Cuál es la cantidad total vendida por producto?

```sql
SELECT 
    p.product_name,
    SUM(f.quantity) AS total_vendido
FROM fact_sales f
JOIN dim_product p ON f.product_id = p.product_id
GROUP BY p.product_name
ORDER BY total_vendido DESC;
```

### 4. ¿Cuál fue la cantidad enviada por mes de envío?

```sql
SELECT 
    sd.month,
    SUM(f.quantity) AS total_enviado
FROM fact_sales f
JOIN dim_ship_date sd ON f.ship_date_id = sd.ship_date_id
GROUP BY sd.month
ORDER BY sd.month;
```

### 5. ¿Cuánto se vendió por tamaño de producto y por estado civil del cliente?

```sql
SELECT 
    p.size,
    c.marital_status,
    SUM(f.salesamount) AS total_vendido
FROM fact_sales f
JOIN dim_product p ON f.product_id = p.product_id
JOIN dim_customer c ON f.customer_id = c.customer_id
GROUP BY p.size, c.marital_status
ORDER BY p.size, c.marital_status;
```
