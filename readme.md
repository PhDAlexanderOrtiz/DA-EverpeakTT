# 📊 Análisis Estadístico y Tratamiento de Outliers — EverPeak (2026-3Q)

> *Contexto analítico: Documentación oficial del flujo metodológico para la limpieza, diagnóstico y tratamiento avanzado de valores atípicos en operaciones comerciales.*

---

## 1. Resumen Ejecutivo (BLUF)
* **Impacto Directivo:** El análisis estadístico de las métricas comerciales de **EverPeak** para el tercer trimestre de 2026 revela la presencia de asimetrías significativas y valores atípicos hacia la derecha en variables transaccionales clave (`price`, `quantity`, `order_value`), en contrastante estabilidad con variables demográficas como la edad del cliente (`customer_age`).
* **Decisión Estratégica:** Para evitar la distorsión analítica sin sacrificar el valor de transacciones extremas o perfiles VIP, se implementó una estrategia de **Winsorization (Capping al percentil 99)**. Esto permite estabilizar las métricas de tendencia central y dispersión, garantizando que los modelos y reportes reflejen fielmente el comportamiento del cliente típico.

---

## 2. Estructura y Datos del Proyecto
* **Origen de los Datos:** Dataset estructurado procesado y alojado en la ruta institucional del entorno de desarrollo: `/datasets/everpeak_clean.csv`.
* **Variables Analizadas (Numéricas Enteras):**
  1. **`price`**: Precio unitario de los productos transaccionados.
  2. **`quantity`**: Cantidad de unidades por orden.
  3. **`order_value`**: Valor monetario total de la orden.
  4. **`customer_age`**: Edad demográfica del cliente.
* **Reproducibilidad:** Entorno configurado en Jupyter Notebook con soporte para Python y procesamiento tabular avanzado.

---

## 3. Metodología y Enfoque Analítico
* **Librerías y Herramientas:** `pandas`, `numpy`, `matplotlib`, `seaborn` para manipulación de datos y visualización diagnóstica.
* **Fases del Protocolo Estadístico:**
  1. **Visión General e Info:** Validación de tipos de datos, conteo de nulos e inspección preliminar mediante `.info()` y `.head()`.
  2. **Estadísticas Descriptivas:** Extracción de métricas de posición y dispersión (media, mediana, mínimos, máximos y cuartiles con `.describe()`).
  3. **Visualización Diagnóstica:** Generación automatizada de histogramas (60 bins) y diagramas de caja (*boxplots*) para identificar sesgos visuales.
  4. **Identificación Formal de Outliers:** Cálculo del Rango Intercuartílico ($IQR = Q3 - Q1$) con verificación de límites superior e inferior ($Q3 + 1.5 \cdot IQR$ y $Q1 - 1.5 \cdot IQR$).
  5. **Clasificación y Tratamiento:**
     * *Drop:* Descarte exclusivo de errores estructurales o valores imposibles.
     * *Keep:* Conservación de transacciones legítimas de clientes de alto valor (VIP).
     * *Winsorization (Capping):* Acotamiento de valores extremos superando el percentil 99 mediante `np.clip()`.
  6. **Estadísticas Post-Tratamiento:** Comparativa métrica antes y después de aplicar el tratamiento de *capping* (`_capped`).

---

## 4. Descubrimientos y Hallazgos Principales (Core)
* **Asimetría Comercial:** Las variables de precio y valor de orden exhiben colas derechas pronunciadas, donde el 25% superior de los datos concentra montos elevados que desvían la media tradicional.
* **Comportamiento Demográfico Estable:** La variable `customer_age` no presenta valores atípicos severos ni sesgos estructurales de consideración, mostrando una distribución simétrica coherente con la base de clientes.
* **Efectividad del Capping:** La aplicación del límite en el percentil 99 estabilizó la varianza en los agregados de transacciones, permitiendo un análisis descriptivo más robusto sin perder la traza de los volúmenes comerciales altos.

---

## 5. Limitaciones Metodológicas
* **Pérdida de Sensibilidad en Extremos Absolutos:** El proceso de *Winsorization* comprime artificialmente la variabilidad de los valores más extremos superiores al $99\%$, lo que puede subestimar el impacto financiero de picos comerciales atípicos excepcionales.
* **Supuestos de Distribución:** El uso del $IQR$ asume propiedades paramétricas y de cuartiles que pueden requerir ajustes adicionales si la naturaleza de las series temporales cambia en trimestres futuros.

---

## 6. Próximos Pasos Estratégicos
* **Segmentación de Clientes VIP:** Aislar los registros superando el percentil 99 en una tabla específica de cuentas clave para campañas de fidelización personalizadas.
* **Modelado Predictivo:** Utilizar las columnas tratadas (`_capped`) como variables base en modelos de predicción de valor de vida del cliente (*CLV*) y propensión de compra.
* **Automatización del Pipeline:** Convertir el script de detección y acotamiento de outliers en una función reutilizable dentro de los pipelines de ingesta de datos de EverPeak.

---

## 🚀 Cómo Ejecutar el Notebook

### Opción 1: Ejecución Local en Jupyter Notebook / VS Code
1. **Clonar el repositorio y situarse en la ruta del proyecto:**
   ```bash
   git clone https://github.com/tu-usuario/everpeak-data-analysis.git
   cd everpeak-data-analysis
   ```
2. **Crear y activar un entorno virtual:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # En Windows: venv\Scripts\activate
   ```
3. **Instalar dependencias requeridas:**
   ```bash
   pip install -r requirements.txt
   ```
4. **Lanzar la interfaz de desarrollo:**
   ```bash
   jupyter lab
   ```
