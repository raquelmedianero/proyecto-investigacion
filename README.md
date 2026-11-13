# Sistema de visión por computador basado en YOLO26l para la detección y clasificación dual de severidad de fisuras en estructuras de concreto según los criterios ACI 201.1R y ACI 224R-01, con garantía conformal del 97 % y cadena de custodia criptográfica conforme a ISO/IEC 27037: caso Aquatech

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Ultralytics](https://img.shields.io/badge/Ultralytics-YOLO26-yellow)](https://github.com/ultralytics/ultralytics)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> **Tesis de Maestría en Inteligencia Artificial**  
> **Autora:** Ing. Raquel Medianero Huari  
> **Institución:** Universidad Nacional de Ingeniería (UNI) - FIIS  
> **Entidad Colaboradora:** Aquatech

## Resumen del Proyecto

Este repositorio contiene el código fuente y el pipeline experimental reproducible para un sistema de visión por computador orientado a la ingeniería forense. El sistema automatiza la detección y clasificación de severidad de fisuras en estructuras de concreto integrando tres pilares fundamentales:

1. **Detección y Segmentación:** Uso del modelo de vanguardia YOLO26l-seg para la extracción precisa de máscaras de fisuras.
2. **Garantía Estadística:** Clasificación dual basada en normativas internacionales (ACI 201.1R y ACI 224R-01) apoyada por Predicción Conforme, asegurando un 97 % de cobertura (α = 0,03) y permitiendo la abstención del modelo en casos ambiguos.
3. **Cadena de Custodia Digital:** Implementación estricta de la norma **ISO/IEC 27037** mediante huellas SHA-256, sellado temporal RFC 3161, firma digital ECDSA-P256, árboles de Merkle y manifiestos C2PA, garantizando la integridad legal de la evidencia (fotografías e informes).

## Estructura del Repositorio

El pipeline experimental está dividido en 5 notebooks de Jupyter ejecutables secuencialmente, diseñados para correr en Google Colab con aceleración GPU.

```text
├── data/
│   ├── crack_mix_v1.zip              # Dataset híbrido (crack-seg + Özgenel) [8432 img]
│   └── verificacion/
│       └── fisuras_verificadas.csv   # Dataset de evaluación pericial Aquatech [144 fisuras]
├── notebooks/
│   ├── 01_exploracion_datos_grietas.ipynb         # Curación, EDA y prevención de data leakage
│   ├── 02_entrenamiento_yolo26l_seg.ipynb         # Entrenamiento con 3 semillas (42, 1337, 2024)
│   ├── 03_evaluacion_deteccion_segmentacion.ipynb # Evaluación intrínseca (mAP, F1, IoU)
│   ├── 04_severidad_conformal_97.ipynb            # Ajuste del cabezal, calibración y evaluación conformal
│   └── 05_custodia_criptografica.ipynb            # Implementación y pruebas activas de la cadena de custodia
├── runs/                                          # Pesos preentrenados y métricas (generados post-entrenamiento)
├── informe/                                       # Resultados consolidados y reportes sellados
├── requirements.txt
└── README.md
```

##  Requisitos e Instalación

Para reproducir los experimentos localmente o en un servidor dedicado, se requiere Python 3.10+ y una GPU compatible con CUDA (recomendado NVIDIA A100 como en el diseño original).

1. Clonar este repositorio:
   ```bash
   git clone https://github.com/tu-usuario/tesis-yolo26l-conformal-custodia.git
   cd tesis-yolo26l-conformal-custodia
   ```

2. Crear un entorno virtual e instalar las dependencias:
   ```bash
   python -m venv venv
   source venv/bin/activate  
   pip install -r requirements.txt
   ```

*Dependencias principales: `ultralytics`, `torch`, `torchvision`, `cryptography`, `opencv-python`, `scikit-image`, `scipy`, `pandas`, `fastapi`, `uvicorn`.*

## Uso y Reproducibilidad

El sistema está diseñado para ser 100% reproducible. Siga el orden de los notebooks:

1. **Datos (NB 01):** Ejecute para verificar la partición estratificada por fotografía y comprobar la ausencia de duplicados.
2. **Entrenamiento (NB 02):** Entrena YOLO26l-seg bajo protocolo estricto. Registra el hash del dataset para mantener la trazabilidad. (Requiere GPU).
3. **Métricas (NB 03):** Genera los reportes de rendimiento de detección sobre el conjunto de prueba congelado (565 imágenes).
4. **Capa Conforme (NB 04):** Aplica la lógica normativa ACI y genera los intervalos de predicción. Incluye un *modo demo* para verificar la matemática de calibración si aún no se cuenta con los pesos finales.
5. **Criptografía (NB 05):** Simula flujos de entrada y prueba las defensas del sistema ante 5 tipos de manipulaciones activas (T1-T5). 

## Metodología y Normativa

- **ACI 201.1R:** Clasificación de anchura de fisura (Fina, Media, Ancha).
- **ACI 224R-01:** Verificación de tolerabilidad según la condición de exposición (ej. aire seco vs. exposición a químicos descongelantes).
- **ISO/IEC 27037:** Directrices para la identificación, recolección, adquisición y preservación de evidencia digital.

## Licencia y Atribución

Este proyecto tiene propósitos académicos. Si utilizas partes de este repositorio en tu investigación, por favor cita la tesis correspondiente.

Los datasets base utilizados para el híbrido (`crack_mix_v1`) provienen de fuentes públicas:
- [Ultralytics crack-seg](https://docs.ultralytics.com/datasets/segment/crack-seg/)
- [Dataset de Özgenel, Mendeley Data V1](https://data.mendeley.com/datasets/jwsn7tfbrp/1)

---
*Desarrollado en el marco del programa de Maestría en Inteligencia Artificial - Universidad Nacional de Ingeniería, Lima, Perú (2026).*
