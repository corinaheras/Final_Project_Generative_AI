# Final_Project_Generative_AI

# 📊 Dashboard Analítico de Inversión Turística - Playas de Ecuador

## 🎯 Descripción del Proyecto

Dashboard interactivo desarrollado para la consultora XYZ con el objetivo de identificar las mejores oportunidades de inversión para eventos musicales y culturales en destinos costeros de Ecuador durante 2026. El análisis combina web scraping, procesamiento de datos, inteligencia artificial generativa y visualización interactiva para evaluar 7 playas ecuatorianas.

**🔗 [Ver Dashboard en Vivo](https://corinaheras.github.io/Final_Project_Generative_AI/)**

---

## 🏖️ Destinos Analizados

- **Villamil Playas**
- **Salinas**
- **Montañita**
- **Puerto López**
- **Ayampe**
- **Manta**
- **Atacames**

---

## 🔬 Metodología

### 1. **Extracción de Datos (Web Scraping)**

- **Fuente**: Booking.com
- **Técnica**: Web scraping automatizado con BeautifulSoup/Selenium
- **Variables extraídas**:
  - Información básica (nombre, ubicación, precio)
  - Ratings y reseñas de usuarios
  - Servicios y amenidades disponibles
  - Capacidad de alojamiento
  - Distancias geográficas

### 2. **Preprocesamiento y Feature Engineering**

#### 2.1 Normalización MinMaxScaler (0-1)
Para asegurar comparabilidad entre variables con diferentes escalas:
- `price` y `avg_price_per_person_per_day`
- `center_distance_km` y `beach_distance_km`
- `rating`
- `top_15_services_count_room`
- `top_15_service_variety_ratio`

#### 2.2 Variables Compuestas Calculadas

**a) Accessibility Index** (Ponderado: Centro 40%, Playa 60%)
```python# 📊 Dashboard Analítico de Inversión Turística - Playas de Ecuador

## 🎯 Descripción del Proyecto

Dashboard interactivo desarrollado para la consultora XYZ con el objetivo de identificar las mejores oportunidades de inversión para eventos musicales y culturales en destinos costeros de Ecuador durante 2026. El análisis combina web scraping, procesamiento de datos, inteligencia artificial generativa y visualización interactiva para evaluar 7 playas ecuatorianas.

**🔗 [Ver Dashboard en Vivo](https://corinaheras.github.io/Final_Project_Generative_AI/)**

---

## 🏖️ Destinos Analizados

- **Villamil Playas**
- **Salinas**
- **Montañita**
- **Puerto López**
- **Ayampe**
- **Manta**
- **Atacames**

---

## 🔬 Metodología

### 1. **Extracción de Datos (Web Scraping)**

- **Fuente**: Booking.com
- **Técnica**: Web scraping automatizado con BeautifulSoup/Selenium
- **Variables extraídas**:
  - Información básica (nombre, ubicación, precio)
  - Ratings y reseñas de usuarios
  - Servicios y amenidades disponibles
  - Capacidad de alojamiento
  - Distancias geográficas

### 2. **Preprocesamiento y Feature Engineering**

#### 2.1 Normalización MinMaxScaler (0-1)
Para asegurar comparabilidad entre variables con diferentes escalas:
- `price` y `avg_price_per_person_per_day`
- `center_distance_km` y `beach_distance_km`
- `rating`
- `top_15_services_count_room`
- `top_15_service_variety_ratio`

#### 2.2 Variables Compuestas Calculadas

**a) Accessibility Index** (Ponderado: Centro 40%, Playa 60%)
```python
accessibility = 0.4 × (1 - center_distance_norm) + 0.6 × (1 - beach_distance_norm)
```
*Rationale*: Proximidad a infraestructura urbana y atractivos naturales facilita logística de eventos.

**b) Hospitality Score** (Ponderado: 50% Sentiment, 50% Rating)
```python
hospitality = 0.5 × sentiment_score_norm + 0.5 × rating_norm
```
*Rationale*: Combina percepción cualitativa (IA sobre reseñas) con métricas cuantitativas (ratings).

**c) Event Potential Index** (Índice Final)
```python
Event_Potential_Index = 
    0.25 × hospitality +
    0.15 × accessibility +
    0.15 × (1 - price_norm) +
    0.20 × service_variety +
    0.25 × capacity
```

**Justificación de Pesos**:
- **25% Hospitalidad**: Experiencia del huésped es crítica para eventos recurrentes
- **25% Capacidad**: Volumen de asistentes determina rentabilidad
- **20% Variedad de Servicios**: Infraestructura para eventos corporativos/culturales
- **15% Accesibilidad**: Facilita llegada de asistentes nacionales/internacionales
- **15% Precio Competitivo**: Balance costo-beneficio para organizadores

### 3. **Inteligencia Artificial Generativa**

#### 3.1 Análisis de Sentimiento (OpenAI GPT-4)
- **Modelo**: `gpt-4o-mini`
- **Input**: Reseñas textuales agregadas por hotel
- **Output**: Sentiment score [-1, 1] donde:
  - `-1`: Muy negativo
  - `0`: Neutral
  - `+1`: Muy positivo
- **Prompt Engineering**: Instrucciones específicas para análisis contextual de hospitalidad

#### 3.2 Generación de Reportes Estructurados
Utilizando **Structured Outputs con Pydantic** para asegurar consistencia:

**a) Reporte de Inversión Final** (`FinalInvestmentReport`)
- Ranking cuantitativo de las 7 ciudades
- Recomendación estratégica justificada
- Análisis de riesgos para la ciudad ganadora

**b) Análisis de Perfil Competitivo** (`MarketCompetitiveAnalysis`)
- Fortalezas y debilidades de cada ciudad vs. mercado
- Posicionamiento estratégico (Líder/Competidor/Promedio/Rezagado)
- Dinámica competitiva general

**c) Análisis Precio-Valor** (`PriceValueAnalysis`)
- Segmentación del mercado (Premium/Value/Economy)
- Clasificación de valor (Best Value/Overpriced/Fair Value)
- Perfiles de cliente objetivo por ciudad
- Gaps y oportunidades de diferenciación

---

## 📈 Componentes del Dashboard

### 1. **Mapa Interactivo**
- Visualización geográfica de hoteles con burbujas proporcionales a capacidad
- Código de colores según Event Potential Index
- Tooltips con información detallada

### 2. **Ranking de Ciudades**
- Tabla comparativa con desglose de componentes
- Indicadores visuales de fortalezas/debilidades
- Métricas normalizadas para comparación directa

### 3. **Análisis Comparativo**
- Gráficos radar para perfiles multidimensionales
- Comparación lado a lado de variables clave
- Distribución de precios por destino

### 4. **Insights Cualitativos**
- Top 5 mejores/peores reseñas por ciudad
- Análisis de sentimiento visualizado
- Recomendaciones estratégicas generadas por IA

### 5. **Análisis de Segmentación**
- Matriz precio-calidad
- Identificación de Best Value destinations
- Oportunidades de posicionamiento

---

## 🛠️ Stack Tecnológico

### Backend & Data Processing
- **Python 3.10+**
- **pandas**: Manipulación de datos
- **scikit-learn**: Normalización (MinMaxScaler)
- **BeautifulSoup/Selenium**: Web scraping

### Inteligencia Artificial
- **OpenAI GPT-4/GPT-4o**: 
  - Análisis de sentimiento
  - Generación de reportes estratégicos
- **Pydantic**: Validación y estructuración de outputs de IA

### Frontend & Visualización
- **HTML5/CSS3/JavaScript**
- **Plotly.js**: Gráficos interactivos
- **Leaflet.js**: Mapas geográficos
- **Chart.js**: Visualizaciones comparativas

### Deployment
- **GitHub Pages**: Hosting estático
- **JSON**: Formato de intercambio de datos

---

## 📊 Hallazgos Principales

### 🥇 Ciudad Recomendada
> *Ver en el dashboard para resultados actualizados basados en datos procesados*

### 💡 Insights Clave
1. **Balance es crítico**: Las ciudades con mejor Event Potential Index no son necesariamente las más caras ni las más baratas, sino aquellas con equilibrio entre capacidad, hospitalidad y precio.

2. **Accesibilidad subestimada**: Destinos con buena conectividad urbana pero lejanía de playa pueden compensar con infraestructura de servicios.

3. **Sentimiento predice capacidad de repetición**: Alta correlación entre sentiment positivo en reseñas y ratings sostenidos en el tiempo.

4. **Segmentación clara del mercado**: Se identifican 3 segmentos distintos (Premium/Value/Economy) con diferentes propuestas de valor.

---

## 📁 Estructura del Proyecto
```
Final_Project_Generative_AI/
├── index.html                 # Dashboard principal
├── data/
│   ├── processed_data.json    # Datos preprocesados
│   ├── ai_insights.json       # Outputs de IA
│   └── map_data.json         # Datos geoespaciales
├── notebooks/
│   ├── 01_web_scraping.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_feature_engineering.ipynb
│   └── 04_ai_analysis.ipynb
├── scripts/
│   ├── scraper.py
│   ├── generate_insights.py
│   └── export_dashboard_data.py
└── README.md
```

---

## 🚀 Reproducibilidad

### Requisitos
```bash
pip install pandas numpy scikit-learn openai pydantic beautifulsoup4 selenium
```

### Configuración API OpenAI
```python
from openai import OpenAI
client = OpenAI(api_key="tu-api-key")
```

### Ejecución
1. **Scraping**: Ejecutar notebooks en orden secuencial
2. **Procesamiento**: Scripts de feature engineering
3. **IA Generativa**: `generate_insights.py` con API key configurada
4. **Export**: `export_dashboard_data.py` para generar JSONs
5. **Deploy**: Push a GitHub Pages

---

## 📚 Referencias Metodológicas

- **Normalización Min-Max**: Scikit-learn Documentation
- **Análisis de Sentimiento con LLMs**: OpenAI Best Practices
- **Structured Outputs**: Pydantic + OpenAI Structured Outputs
- **Web Scraping Ético**: Respeto a robots.txt y rate limiting

---

## 👥 Autores

*Proyecto desarrollado para el Módulo de Inteligencia Artificial Generativa*

---

## 📄 Licencia

Proyecto académico - Consultora XYZ (simulado)

---

## 🔮 Futuras Mejoras

- [ ] Integración de datos climáticos por temporada
- [ ] Análisis de tendencias temporales (booking patterns)
- [ ] Modelo predictivo de ocupación para eventos
- [ ] Dashboard en tiempo real con APIs de Booking
- [ ] Sistema de recomendación personalizado por tipo de evento

---

**📌 Nota**: Este proyecto demuestra la aplicación práctica de IA Generativa en análisis de negocios, combinando técnicas tradicionales de data science con capacidades avanzadas de LLMs para generación de insights estratégicos.