# Inspección Forense de Rajaduras con YOLO11 en Edge AI

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-EE4C2C.svg)](https://pytorch.org/)
[![Ultralytics YOLO11](https://img.shields.io/badge/YOLO-11-yellow.svg)](https://github.com/ultralytics/ultralytics)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

> **Tesis de Maestría en Inteligencia Artificial - Universidad Nacional de Ingeniería (UNI)**
> **Autora:** Ing. Raquel Medianero Huari
> **Periodo:** Abril 2026 - Febrero 2027

## Descripción del Proyecto

Este repositorio contiene el código fuente y la investigación experimental para un **Sistema de Inspección Forense de Salud Estructural (SHM)** enfocado en la infraestructura de la empresa **Aquatech**. 

Las filtraciones por rajaduras en estructuras aceleran la corrosión y el fallo estructural. Este proyecto resuelve la subjetividad y lentitud de las inspecciones manuales mediante un sistema de visión por computadora implementado en el borde (*Edge AI*), capaz de procesar imágenes tomadas desde *smartphones* en condiciones de campo.

El sistema no solo detecta, sino que **segmenta, mide en milímetros físicos y acota la incertidumbre** de las rajaduras bajo los estándares del ACI 224R-01.

## Características Principales

*   **Segmentación de Instancias (SOTA):** Utiliza YOLO11-seg optimizado para evitar el colapso topológico en microfisuras capilares mediante *Feature Engineering* morfológico.
*   **Cuantificación Fotogramétrica Exacta:** Aproximación matemática mediante Transformada del Eje Medial (MAT) para arrojar medidas en $mm$ (ancho y longitud).
*   **Incertidumbre Calibrada (Conformal Prediction):** Cada medición de ancho se acompaña de un intervalo de cobertura estadísticamente garantizado, esencial para peritajes forenses.
*   **Explicabilidad (XAI):** Generación de superposiciones visuales (overlays) y mapas de activación para justificar el diagnóstico ante los ingenieros peritos.
*   **Robustez Multi-Dispositivo:** Entrenado con *Domain-Aware Augmentation* y evaluado bajo un protocolo *leave-one-device-out* para garantizar su funcionamiento sin importar el smartphone utilizado.

## Conjuntos de Datos

1.  **CCSS-DATA-V4:** Dataset público con 670 imágenes RGB (capturadas con 4 sensores distintos: Canon, Xiaomi, ZED, Samsung) para validación de degradación de dominio.
2.  **Aquatech Field Dataset:** Dataset propio con más de 100 imágenes tomadas en las instalaciones operativas, con reglas de escala y lecturas de fisurómetro como referencia física (*Ground Truth* métrico).

## Instalación y Uso Rápido

### Requisitos previos
Se recomienda utilizar un entorno virtual con Python 3.10+.

```bash
# 1. Clonar el repositorio
git clone https://github.com/raquelmedianero/proyecto-investigacion.git



### Ejecutar Inferencia de Prueba

Para probar el motor de segmentación y medición geométrica sobre una imagen de prueba:

```bash
python src/inference.py --source data/samples/grieta_muro_01.jpg --weights models/yolo11_aquatech_best.pt --calibrate True
```
*El script generará un reporte visual (`.png`) en la carpeta `outputs/` incluyendo la máscara de segmentación, el cálculo en $mm$ y el diagnóstico preliminar.*


## Pipeline de Investigación 

1.  **Ingeniería de Datos:** Auditoría del dataset, particionamiento fijo con 5 semillas aleatorias y aumento fotométrico dirigido.
2.  **Modelado Edge AI:** Entrenamiento de YOLOv8-seg (línea base) vs YOLO11-seg (`n` y `s`).
3.  **Medición y Calibración:** Desarrollo del motor fotogramétrico (Píxel a Milímetro).
4.  **Conformidad y XAI:** Integración de los intervalos conformes (90% de cobertura empírica) y reportes automatizados.


---
