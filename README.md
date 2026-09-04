# Expediente EDA: NovaMarket

Taller individual de **Análisis Exploratorio de Datos (EDA)** para la asignatura de Machine Learning. El objetivo no es "hacer gráficos", sino **investigar la confiabilidad de los datos** de una cadena de retail ficticia y **contrastar con evidencia** las afirmaciones que la gerencia da por ciertas.

- **Autora:** Areliz Isla Treuque
- **Sección:** MLY1101
- **Fecha:** 29-08-2026
- **Notebook:** `MLY1101_001V_T03_IslaAreliz.ipynb`

---

## El caso

NovaMarket es una cadena de retail omnicanal con ocho sucursales que vende por **Tienda, Web y App**. Antes de invertir en un proyecto de Machine Learning, la empresa necesita saber si sus datos son confiables y si las conclusiones que usa la gerencia están realmente respaldadas por evidencia.

El trabajo se realiza sobre **25.000 transacciones del año 2025** (`dataset_novamarket_eda.csv`, 20 columnas). La tarea consiste en investigar los datos, formular preguntas, contrastar afirmaciones y distinguir entre **errores reales**, **fenómenos genuinos** y **situaciones donde la evidencia no alcanza** para concluir.

---

## Reglas del taller

- Seguir las etapas del notebook en orden.
- **No** entrenar ningún modelo.
- **No** usar herramientas automáticas de EDA (`ydata-profiling`, `Sweetviz`, `AutoViz`, etc.).
- Se permiten `pandas`, `numpy`, `matplotlib`, `seaborn` y librerías habituales.
- Máximo **7 visualizaciones** en todo el trabajo; cada una debe responder una pregunta concreta.
- Toda conclusión debe estar respaldada por evidencia calculada desde el dataset.
- No eliminar valores extremos o registros sospechosos sin investigarlos primero.
- Mantener visibles todos los resultados al entregar.

---

## Requisitos y ejecución

**Dependencias:**

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

**Datos:** el archivo `dataset_novamarket_eda.csv` debe estar en la misma carpeta que el notebook (la ruta está definida en la variable `ARCHIVO` de la primera celda de código).

**Ejecutar:**

```bash
jupyter notebook MLY1101_001V_T03_IslaAreliz.ipynb
```

Luego ejecutar las celdas en orden (menú *Run All* o `Shift+Enter` celda por celda). El notebook fue desarrollado en Google Colab, por lo que también puede subirse directamente a Colab junto con el CSV.

---

## Diccionario de datos

| Variable | Descripción |
|---|---|
| `id_transaccion` | Identificador declarado de la transacción |
| `fecha_hora` | Fecha y hora del registro |
| `sucursal` | Sucursal asociada |
| `canal` | Tienda, Web o App |
| `cliente_id` | Identificador del cliente |
| `segmento_cliente` | Estándar, Premium o Convenio Empresa |
| `edad_cliente` | Edad registrada |
| `antiguedad_meses` | Antigüedad del cliente en meses |
| `categoria` | Categoría comercial registrada |
| `producto_id` | Identificador de producto |
| `precio_unitario` | Precio unitario registrado en CLP |
| `cantidad` | Unidades incluidas en la transacción |
| `descuento_pct` | Porcentaje de descuento aplicado |
| `campania` | Campaña comercial vigente |
| `medio_pago` | Medio de pago |
| `dias_entrega` | Días hasta la entrega; 0 = compra en tienda |
| `satisfaccion` | Evaluación del cliente de 1 a 5; puede faltar |
| `devolucion` | 1 si la venta terminó en devolución |
| `monto_venta` | Monto registrado de la venta en CLP |
| `monto_reembolso` | Monto posteriormente reembolsado en CLP |

---

## Estructura del notebook

El análisis está organizado en cinco etapas que van del reconocimiento inicial hasta el veredicto sobre la preparación de los datos para modelar.

### 1. Primer contacto con los datos
Exploración inicial sin limpiar todavía: dimensiones, `info()`, estadísticos descriptivos, revisión de nulos y duplicados. Se verifica la **integridad de la venta** recomputando `monto_calculado = precio_unitario × cantidad × (1 − descuento_pct/100)` y comparándolo con `monto_venta` para detectar discrepancias. Se plantean **3 preguntas/sospechas iniciales** (descuentos sin campaña, medios de pago por segmento, y coherencia entre devolución y reembolso) y se registran **al menos 3 aspectos** que merecen investigación.

### 2. Investigación de afirmaciones de la gerencia
Cada afirmación se clasifica como **SOSTENIDA / REFUTADA / NO CONCLUYENTE**, con estrategia previa, evidencia y justificación:

- **Afirmación A** — "A mayor descuento, mayor monto de venta" → analizada con correlación de Pearson y ticket promedio por tramos de descuento.
- **Afirmación B** — "Los clientes Premium están más satisfechos que los Estándar" → promedios, medianas y boxplot por segmento.
- **Afirmación C** — "La caída de transacciones del último trimestre fue general" → evolución temporal mensual segmentada por sucursal.
- **Afirmación D** — "Los valores extremos de monto son errores y deben eliminarse" → boxplot, umbral IQR y verificación de consistencia interna de los outliers.
- **Afirmación E** — "Los datos faltantes de satisfacción son aleatorios" → análisis de los nulos por canal, devolución y días de entrega (patrón MNAR).

### 3. Hallazgos que la gerencia no pidió
Dos hallazgos propios surgidos del análisis, sustentados en evidencia calculada del dataset.

### 4. Intenta demostrar que estás equivocado
Ejercicio de pensamiento crítico sobre el hallazgo más importante (preferencia de medios de pago por segmento): se formula una **hipótesis**, se busca **contraevidencia** activamente y se toma una **decisión final** (mantener o descartar la hipótesis).

### 5. ¿Está este dataset listo para Machine Learning?
Cierre sin entrenar modelos. Se identifican los **tres riesgos principales** a resolver antes de modelar, entre ellos:
- Datos faltantes de `satisfaccion` con patrón **MNAR** (concentrados en devoluciones), que sesgarían el modelo.
- Discrepancias en `monto_venta` frente al monto calculado, que comprometen la integridad de la variable objetivo.

---

## Notas de reproducibilidad

- El notebook **crea columnas auxiliares** durante el análisis (`monto_calculado`, `diferencia_monto`, `tramo_descuento`, `mes`, `satisfaccion_nula`). Al ejecutar en orden se generan sin problema; ejecutar celdas de forma aislada puede provocar errores de columnas inexistentes.
- Los resultados y gráficos ya vienen renderizados en el notebook entregado; volver a ejecutar *Run All* debería reproducirlos siempre que el CSV esté disponible.
