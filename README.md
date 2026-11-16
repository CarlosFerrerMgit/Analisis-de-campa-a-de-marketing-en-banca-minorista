# 🐍 Proyecto EDA con Python: Análisis de campaña de marketing en banca minorista

> **Módulo 7: Python for Data**  
> **Python 3.12+ (Entorno de desarrollo en VS Code)**  
> **Fecha: Noviembre 2026** 

## 📌 1. Descripción del Proyecto
Este proyecto consiste en un **Análisis Exploratorio de Datos (EDA)** completo sobre las campañas de marketing de una entidad financiera portuguesa para la suscripción de depósitos a plazo fijo (`y`). 

Partiendo de dos fuentes de datos desacopladas, el objetivo ha sido auditar, limpiar y cruzar la información, identificar qué factores impulsan realmente la conversión (situada en una media base del **11,27%**) y validar los patrones mediante pruebas estadísticas para proponer mejoras operativas al banco.

## 🛠️ 2. Metodología y Pasos Seguidos

### A. Pasos Seguidos durante el Proyecto
1. **Ingesta y Consolidación Multihoja:** Carga automatizada de las tres pestañas anuales del libro `customer-details.xlsx` y cruce relacional con `bank-additional.csv` a través de las claves `id_` e `ID`, consolidando 43.000 registros únicos sin pérdida de información.
2. **Saneamiento y Normalización de Tipos:** Conversión de decimales con formato europeo en indicadores macroeconómicos, estandarización de cadenas de texto y transformación de fechas textuales en español (`2-agosto-2019`) al estándar `datetime64` y enteros puros `Int64`.
3. **Imputación No Destructiva e Ingeniería de Variables:** Tratamiento de valores nulos preservando el 100% de la muestra mediante medianas condicionadas por grupo profesional (`age`) y periodo temporal (`euribor3m`), además de la creación de variables derivadas clave (`target_numeric`, `age_group`, `total_children`, `contacted_before`).
4. **Análisis Descriptivo y Validación Estadística:** Exploración visual siguiendo un hilo lógico de lo macroeconómico a lo operativo, respaldando cada hallazgo visual con contrastes de hipótesis formales en `scipy.stats`.

### B. Competencias Técnicas Demostradas
* **Manipulación con Pandas & NumPy:** Uso eficiente de operaciones vectorizadas, agrupaciones complejas (`.groupby().agg()`, `.transform()`), discretización de variables (`pd.cut()`) y gestión de tipos *nullable* (`Int64`).
* **Validación Estadística (`scipy.stats`):** Aplicación fundamentada de pruebas según la naturaleza de cada variable: Correlación Biserial-Puntual, Test Chi-cuadrado ($\chi^2$) de Independencia, Test no paramétrico U de Mann-Whitney, Correlación de Rangos de Spearman y Test *t* de Student.
* **Visualización Analítica (`Matplotlib` & `Seaborn`):** Diseño de gráficos limpios orientados a negocio.

> 📄 *Para consultar el desarrollo completo del código, la metodología de limpieza y todas las pruebas estadísticas, puedes revisar el documento [INFORME TECNICO DATAPROJECT.pdf](docs/INFORME%20TECNICO%20DATAPROJECT.pdf) y los cuadernos de la carpeta [notebooks/](notebooks/).*

## 📁 3. Estructura del Proyecto
```text

├── data/
│   ├── clean/
│   │   └── bank_clean.csv               # Dataset consolidado y listo para análisis
│   └── raw/
│       ├── bank-additional.csv          # Datos originales de llamadas y contexto macroeconómico
│       └── customer-details.xlsx        # Datos originales
├── docs/
│   ├── figuras/
│   │   ├── grafico 01.png               # Distribución de la variable objetivo ('y')
│   │   ├── grafico 02.png               # Conversión según tramos del Euribor 3M
│   │   ├── grafico 03.png               # Tasa de conversión por rango de edad
│   │   ├── grafico 04.png               # Boxplots de ingresos y visitas web vs contratación
│   │   ├── grafico 05.png               # Conversión según resultado de campaña previa
│   │   ├── grafico 06.png               # Curva de densidad y duración de llamadas
│   │   └── grafico 07.png               # Fatiga comercial por número de contactos
│   ├── INFORME TECNICO DATAPROJECT.pdf  # Memoria técnica completa del proyecto
│   └── Instrucciones y variables.docx   # Requisitos del proyecto y diccionario de variables
├── notebooks/
│   ├── 01_eda_inicial.ipynb             # Fase ETL
│   └── 02_eda_descriptivo.ipynb         # Análisis descriptivo
└── READMe.md                            # Documentación principal del repositorio
```

## 📊 4. Hallazgos Visuales Clave

A continuación se muestran los cuatro indicadores con mayor impacto en la tasa de conversión (frente al **11,27%** de media global del banco)

| 1. Impacto del Ciclo Económico (Euribor 3M) | 2. Disparidad Demográfica por Edad |
| :---: | :---: |
| ![Conversión según Euribor](docs/figuras/grafico%2002.png) | ![Conversión por Edad](docs/figuras/grafico%2003.png) |
| *Cuando los tipos bajan (<3%), la suscripción escala hasta el **44,6%**.* | *Los **Jubilados (>65 años)** alcanzan un **46,0%**, cuadruplicando la media.* |

| 3. Efecto "Cliente Caliente" (`poutcome`) | 4. Fatiga Comercial por Contactos |
| :---: | :---: |
| ![Conversión por Poutcome](docs/figuras/grafico%2005.png) | ![Fatiga Comercial](docs/figuras/grafico%2007.png) |
| *El éxito previo dispara la conversión hasta el **65,3%**.* | *A partir de la **4.ª llamada**, la eficacia cae por debajo de la media.* |

## 🎯 5. Conclusiones y Plan Estratégico Operativo

### A. Conclusiones Principales

Tras completar el recorrido analítico y validar cada patrón con pruebas estadísticas, la conclusión central es que realizar llamadas telefónicas masivas e indiscriminadas resulta ineficiente (tasa base de conversión del **11,27%**), invirtiendo gran parte del esfuerzo del *call center* en perfiles con baja probabilidad de contratación.

El éxito de la campaña no depende de llamar a clientes con mayores ingresos ni de insistir repetidamente, sino de combinar tres factores clave: un **entorno económico de tipos de interés contenidos**, un **perfil demográfico senior o jubilado** con ahorro acumulado, y una **interacción telefónica de calidad**.

### B. Recomendaciones Operativas para el Banco

1. **Límite máximo de 3 llamadas por cliente:** Configurar un descarte automático tras el tercer intento fallido. A partir de la cuarta llamada la efectividad cae por debajo de la media global, consumiendo horas operativas y generando rechazo.
2. **Priorizar historial previo y edad sobre el nivel de ingresos:** Enrutar primero a los clientes con éxito en campañas anteriores (`poutcome == 'success'`, **65,32%** de conversión) y al segmento **Jubilados (>65 años, 46,0%)**[cite: 21, 22]. Se recomienda eliminar el filtro por salario (`Income`), ya que no influye estadísticamente en la compra.
3. **Sincronizar las campañas con el ciclo del Euribor:** Concentrar el presupuesto y las llamadas masivas en periodos con tipos de interés bajos o descendentes (<3%), momento en que el depósito resulta más atractivo frente a otras alternativas.
4. **Fomentar llamadas de asesoramiento frente a llamadas rápidas:** Evitar evaluar a los operadores únicamente por el volumen de llamadas por hora. Las suscripciones exitosas presentan una mediana de duración de **449 segundos (~7,5 min)** frente a los 163 s de los rechazos, por lo que tras superar los 3-4 minutos de conversación se debe priorizar una explicación detallada para asegurar la contratación.

 ## 🤝 Contribuciones

 Las contribuciones, sugerencias y mejoras son bienvenidas. 
 
 ## 👤 Autor
 
 Carlos A. Ferrer Mendoza — Ingeniero Electrónico | MBA y Alumno del Máster en Data & Analytics (ThePower Business School).
 www.linkedin.com/in/carlos-ferrer-mendoza-835294292