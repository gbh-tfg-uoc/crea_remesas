# 🏦 Generador de Transacciones Sintéticas para Análisis AML (Remesadoras)

Este proyecto contiene un script en formato notebook (`CreaCSV.ipynb`) que permite generar automáticamente ficheros `.csv` simulando transacciones realizadas por agentes y sujetos obligados, con múltiples etiquetas de riesgo típicas del blanqueo de capitales. Es una herramienta útil para analistas, desarrolladores y auditores de blanqueo de capitales.

---

## 📦 Estructura de carpetas

📁 fuentes/
┣ municipios.csv → Listado oficial de municipios españoles
┗ CCAA.csv → Codificación CCAA ↔ Código postal

📁 ejemplos/
┗ Agente_X__Entidad_Y.csv → Ejemplos reales generados

📄CreaCSV.ipynb → Notebook principal


---

## 🧾 ¿Qué hace este código?

1. **Carga y filtra el fichero JSON oficial del registro EBA**:
   - Filtra entidades de tipo PSD_AG activas y con residencia en España.
   - Expande los atributos y limpia nombres y códigos.

2. **Enriquece los datos** con los siguientes ficheros (desde `/fuentes/`):
   - `municipios.csv`: proporciona nombre, provincia y código postal.
   - `CCAA.csv`: enlaza el código postal con la comunidad autónoma correspondiente.

3. **Asigna información geográfica y normaliza registros**:
   - Une información de localidad y región usando matching difuso (`RapidFuzz`).

4. **Genera CSVs de transacciones sintéticas**:
   - Cada archivo representa la operativa de un agente.
   - Incluye patrones de riesgo como fraccionamiento, nodos concentradores, PEPs, documentos sospechosos, etc.

5. **Simula contextos realistas**:
   - Genera entre 300 y 1200 transacciones por archivo.
   - Imita horarios de envío, flujos, y variabilidad natural.
   - Las entidades generadas son sintéticas y anónimas.

---

## 📄 Estructura del CSV generado

Cada fichero `.csv` contiene columnas como:

- **Datos de ordenante**: nombre, NIE, país, fecha nacimiento
- **Datos del beneficiario**: nombre, país destino
- **Información de la transacción**: importe, fecha, hora, estado
- **Etiquetas de riesgo**:
  - `es_Agente`: si el ordenante es el agente
  - `es_PEP`: persona políticamente expuesta
- **Metadatos AML**:
  - `ENT_TOW_CIT_RES`, `ENT_COD_PAR_ENT`, `ENT_NAT_REF_COD`: ciudad, sujeto obligado, agente

---

## 🛠 Manual de uso

### 1. Preparación

Antes de ejecutar el cuaderno:

- Asegúrate de tener los siguientes ficheros:
  - Fichero JSON del **registro EBA** (no incluido por tamaño, se descarga desde [EBA](https://euclid.eba.europa.eu/register/))
  - `municipios.csv` y `CCAA.csv` en la carpeta `/fuentes/`

### 2. Instalación de librerías

###```bash
pip install pandas numpy matplotlib seaborn plotly faker rapidfuzz openpyxl 


### 3. Ejecución

    Abre el notebook CreaCSV.ipynb.

    Ejecuta todas las celdas. El primer fichero que solicita el programa es el json de la EBA, en segundo lugar es necesario el fichero 
    municipios y por último el fichero CCAA

    Introduce el número de ficheros .csv a generar cuando se solicite.

    Los ficheros se guardan automáticamente en el directorio de trabajo con nombres como:

Sujeto_Obligado3__Agente5.csv

Cada uno simula operativa completa con estructuras sospechosas o normales.

---

## 📄 Licencia

Este proyecto se distribuye bajo la Licencia Creative Commons. Consulta el archivo LICENSE para más detalles.

Desarrollado como parte de un TFG en prevención del blanqueo de capitales. Para soporte o dudas, contactar a través del repositorio o con 📬 gborr@uoc.edu.
