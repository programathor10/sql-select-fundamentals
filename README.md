# 📊 SQL Select Fundamentals

Documentación correspondiente al ejercicio práctico sobre consultas básicas en SQL, enfocada en buenas prácticas de rendimiento y legibilidad de datos.

---

### 1. ¿Por qué es mala práctica usar `SELECT *` en producción?

Usar `SELECT *` en entornos productivos se considera una mala práctica por los siguientes motivos:

* **Rendimiento:** Traer todas las columnas de una tabla que tiene decenas de campos consume memoria, CPU y ancho de banda de red innecesarios cuando solo necesitamos unos pocos datos puntuales.
* **Seguridad:** Al traer todo sin filtrar, corremos el riesgo de exponer información sensible o privada (como contraseñas encriptadas, tokens o datos personales) hacia la aplicación o reporte que consume la consulta.
* **Mantenibilidad:** Si en el futuro alguien agrega, elimina o reordena columnas en la tabla original, las aplicaciones o reportes que dependan de ese `SELECT *` pueden fallar o mostrar datos incorrectos.

**Ejemplo:**
```sql
-- ❌ Mala práctica: trae absolutamente todo
SELECT * FROM usuarios;

-- ✅ Buena práctica: solicita únicamente lo necesario
SELECT id, nombre, email FROM usuarios;
```

---

### 2. ¿Por qué son importantes los alias para un stakeholder no técnico?

Los alias (`AS`) permiten renombrar columnas de forma temporal en el resultado de una consulta. Esto es fundamental cuando compartimos reportes con áreas no técnicas (como Finanzas, Marketing o Gerencia), ya que traduce los nombres técnicos de la base de datos —que suelen estar en inglés y en formato `snake_case`— a un lenguaje de negocio claro y directo.

**Ejemplo aplicado a Finanzas:**
En una base de datos, el monto de una venta suele llamarse `total_amount`. Para alguien de Finanzas, ese nombre es técnico y ambiguo (no aclara si incluye impuestos o en qué moneda está). Con un alias lo volvemos inmediatamente comprensible:

```sql
SELECT 
    order_id AS "N° de Factura",
    total_amount AS "Monto Total Facturado (USD)"
FROM ventas;
```

De esta manera, quien recibe el reporte entiende al instante qué representa cada columna sin necesidad de conocer la estructura interna de la base de datos.
