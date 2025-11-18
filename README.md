# 📊 Dashboard Analítico de Inversión Turística - Playas de Ecuador

## 🎯 Descripción del Proyecto

Este proyecto presenta un dashboard interactivo desarrollado para la consultora ficticia XYZ, con el objetivo de identificar las mejores oportunidades de inversión para un promotor turístico que planea organizar eventos musicales y culturales en destinos costeros de Ecuador en 2026.

El análisis combina **Web Mining (Scraping)**, **Ingeniería de Características** y **Análisis de IA Generativa (GPT-4o)** para evaluar siete playas clave, determinando su viabilidad basada en dos pilares: **Capacidad de Alojamiento** y **Nivel de Hospitalidad**.

## 🔗 Ver Dashboard en Vivo

[Enlace al Dashboard](https://corinaheras.github.io/Final_Project_Generative_AI/)

## 🏖️ Destinos Analizados

- Atacames
- Ayampe
- Manta
- Montañita
- Puerto López
- Salinas
- Villamil Playas

---

## 📈 Reporte de Análisis Estratégico

### 1. Estrategia de Recolección de Datos (Web Mining)

Para evaluar la **"Capacidad"** y **"Hospitalidad"**, se determinó que la plataforma **Booking.com** es el proxy más preciso del mercado de alojamiento.

**Fuente de Datos:** Booking.com (Resultados de búsqueda para las 7 ciudades)

**Herramienta de Extracción:** Se desarrolló un scrapper personalizado en Python (`scrapper/booking_scrapper_refactor_nuevo.py`) utilizando:
- **Selenium** (para navegación e interacción con elementos dinámicos)
- **BeautifulSoup** (para el parseo de HTML)

**Variables Clave Recolectadas:**
- `title`, `description`: Para extraer capacidad (adultos, niños) y servicios
- `price`: Costo base
- `rating`: Métrica de hospitalidad (1-10)
- `reviews`: Texto crudo de reseñas (para análisis de sentimiento)
- `location`, `distance`, `beach_distance`: Datos para el índice de accesibilidad
- `services`: Amenidades clave

---

### 2. Metodología de Análisis y Jerarquización

El análisis se estructuró en dos fases: un **modelo cuantitativo** para puntuar la capacidad y un **modelo cualitativo (IA)** para puntuar la hospitalidad.

#### 2.1. Modelo Cuantitativo: El "Event Potential Index"

Para crear un ranking justo, se normalizaron las variables clave usando **MinMaxScaler** (a un rango de 0 a 1) y se construyó un índice ponderado.

##### a) Accessibility Index (Ponderado: Centro 40%, Playa 60%)

**Rationale:** La logística del evento requiere cercanía a infraestructura urbana (centro) y al atractivo principal (playa).
```python
# 1.0 = Máxima accesibilidad (cerca de ambos)
accessibility = 0.4 * (1 - center_distance_norm) + 0.6 * (1 - beach_distance_norm)
```

##### b) Hospitality Score (Ponderado: 70% Sentiment, 30% Rating)

**Rationale:** La percepción cualitativa (sentimiento en reseñas) es un predictor más fuerte de la experiencia real que el rating numérico de la plataforma.
```python
hospitality = 0.5 * sentiment_score_norm + 0.5 * rating_norm
```

##### c) Event Potential Index (EPI) - El Score Final

**Rationale:** Se asigna el mayor peso a la Capacidad y Hospitalidad, ya que son los pilares del requerimiento del cliente.
```python
Event_Potential_Index = (
    0.25 * hospitality +
    0.25 * capacity_norm +
    0.20 * service_variety_norm +
    0.15 * accessibility +
    0.15 * (1 - avg_price_per_person_per_day_norm)  # Precio competitivo
)
```

#### 2.2. Modelo Cualitativo: IA Generativa (GPT-4o y Pydantic)

Los números (como un rating de 8.5) no explican el **por qué** de la experiencia. Se utilizó **IA Generativa** para analizar el texto crudo de las reseñas (`reviews`) y extraer insights estratégicos.

##### a) Análisis de Sentimiento Cuantitativo (Hospitalidad)

- Se diseñó un prompt que instruye a `gpt-4o-mini` para actuar como un analista cuantitativo
- El modelo asignó un score de **-1 (Negativo)** o **+1 (Positivo)** a cada reseña
- El `sentiment_score_norm` final para cada ciudad se calculó usando la fórmula:
```
  (Positivos - Negativos) / (Total de Reseñas)
```
  Normalizando el resultado entre -1 y +1

##### b) Generación de Reportes Estratégicos (Structured Outputs)

Se utilizaron **Esquemas Pydantic** para forzar a la IA a devolver respuestas en un formato JSON estructurado y predecible:

- **`HospitalityRankingReport`**: Obligó a la IA a calcular el score de hospitalidad por ciudad, identificar el principal `key_improvement_area` (ej. "Ducha/Agua") y analizar los riesgos transversales de la región

- **`FinalInvestmentReport`**: El esquema final donde la IA actúa como Consultor Senior. Toma todos los scores cuantitativos (EPI, hospitalidad, capacidad) y genera una justificación cualitativa para la recomendación de inversión, identificando la `top_recommendation_city` y sus `key_risks`

---

### 3. Resultados y Visualización (El Dashboard)

El resultado de todo el análisis se consolidó en un único archivo, `result.json`, que actúa como la base de datos para el dashboard interactivo (`index.html`).

**El dashboard incluye:**

- **Mapa Interactivo (Leaflet.js)**: Muestra la dispersión geográfica de los alojamientos, con colores basados en el Event Potential Index

- **Ranking Cuantitativo (Chart.js)**: Un gráfico de barras que jerarquiza las 7 ciudades según el Score Final

- **Análisis de Componentes (Plotly.js)**: Gráficos radar que descomponen el score de cada ciudad, permitiendo comparar visualmente sus fortalezas (ej. "Manta: Alta Capacidad" vs. "Ayampe: Alta Hospitalidad")

- **Insights de IA (HTML)**: Muestra la justificación estratégica, los riesgos clave y el plan de acción generado por `gpt-4o`

- **Nubes de Palabras (WordCloud)**: Visualización de los temas positivos y negativos más frecuentes extraídos de las reseñas

---

### 4. Conclusiones y Recomendación Final

*(Esta sección debe ser llenada con los resultados finales del notebook)*

**Recomendación de Inversión (Ejemplo):**

Basado en el **Event Potential Index (EPI)**, la ciudad recomendada para la inversión es **[Ciudad Ganadora]**. Aunque **[Ciudad #2]** presenta un mayor `hospitality_score`, la capacidad logística y variedad de servicios de **[Ciudad Ganadora]** la convierten en la opción más robusta y escalable para un evento masivo en 2026.

**Riesgos Clave Identificados por la IA:**

El principal riesgo transversal en la región es **[Riesgo Negativo, ej. "Calidad de Ducha/Agua"]**. Se recomienda al promotor turístico incluir en sus negociaciones con hoteleros locales un plan de mejora de infraestructura básica para mitigar este riesgo.

---

## 🛠️ Stack Tecnológico

- **Data Scraping**: Python, Selenium, BeautifulSoup4
- **Data Processing**: Python, Pandas, Numpy, Scikit-learn
- **IA Generativa**: OpenAI (API), Pydantic (Structured Outputs)
- **Visualización**: HTML5, CSS3, JavaScript, Plotly.js, Leaflet.js, Chart.js
- **Deployment**: GitHub Pages

---

## 📁 Estructura del Proyecto
```
Final_Project_Generative_AI/
├── index.html                  # El Dashboard interactivo (frontend)
├── result.json                 # El archivo de datos final que consume el dashboard
├── notebooks/
│   └── IA_Generativa_Proyecto_Final.ipynb  # Notebook con todo el análisis
├── scrapper/
│   └── booking_scrapper_refactor_nuevo.py  # Script de Web Scraping
└── README.md
```

---

## 🚀 Reproducibilidad

1. **Configurar Entorno**: Instalar dependencias (Pandas, Scikit-learn, OpenAI, Pydantic, Selenium)
2. **API Key**: Configurar la variable de entorno `OPENAI_KEY`
3. **Ejecutar Notebook**: Correr `IA_Generativa_Proyecto_Final.ipynb` de principio a fin. Esto generará el `result.json`
4. **Visualizar**: Abrir `index.html` en un navegador

---

## 👥 Autores

Proyecto desarrollado para el Módulo de **Inteligencia Artificial Generativa**.

- **Corina Montero Heras**
- **Leopoldo Zumba Soliz**

---
