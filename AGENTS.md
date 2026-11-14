# AGENTS.md

Proyecto integrador de Machine Learning (Universidad del Pacífico, 2026-2): detección de intrusiones multiclase
sobre RT-IoT2022, comparando KNN, árbol de decisión, bosque aleatorio y red neuronal.

## Antes de empezar
Lee estos archivos para tener el contexto completo (están en `.gitignore`, solo existen en local):
- `historial_conversacion.txt`: lo que se ha analizado y decidido hasta ahora.
- `modificaciones_paper.txt`: cambios pendientes del paper, que se aplican al final.

Si no existen, el contexto mínimo está en este archivo.

## Archivos
- `20262_UP_ML_Porras_Pando_Delgado_PI_E1 (1).pdf`: paper de la Entrega 1. No modificarlo.
- `20262_UP_ML_Proyecto_Integrador_Rúbrica_Entrega_1_v1_G01.xlsx`: rúbrica de la E1 (19/20).
- `RT_IOT2022`: dataset original (123 117 filas).
- `preparacion_datos.ipynb`: limpieza y selección de variables. Genera `RT_IOT2022_preparado.csv` (117 922 filas),
  que no se versiona: hay que ejecutar este notebook antes que los demás.
- `redes_neuronales.ipynb`: red neuronal (MLPClassifier). Hay que reentrenarlo con el CSV nuevo.

## Reglas
- Solo scikit-learn (nada de TensorFlow, PyTorch ni imbalanced-learn).
- Kernel: venv `~/Documents/mining_semana_12/.venv`.
- Trabajar paso a paso: no añadir celdas ni código a los notebooks si no se pide. El usuario prefiere recibir el
  código en el chat, separado por celdas y con su markdown, para pegarlo él.
- No modificar el PDF del paper; anotar los cambios en `modificaciones_paper.txt`.
- Responder en español.

## Decisiones vigentes
- Duplicados: se eliminan solo los exactos (todas las columnas). Tras quitar `puerto_origen` y otras
  variables, las filas que coinciden en features se conservan (temporalidades distintas, no duplicados
  estrictos; criterio acordado con el profesor).
- Clases foco del análisis: `NMAP_FIN_SCAN` (28 registros) y `Metasploit_Brute_Force_SSH` (36).
- Los 4 modelos se entrenan con las 12 clases, con la misma partición 70/15/15 estratificada
  (`random_state=42`) y el mismo preprocesamiento.
- Métricas prometidas en el paper: recall por clase, macro-F1 y matriz de confusión, contrastadas con la
  exactitud global. Los hiperparámetros se eligen por macro-F1 en validación.
- Consecuencia: puede haber vectores idénticos en train/val/test; `DOS_SYN_Hping` sigue dominando (~76 %).
