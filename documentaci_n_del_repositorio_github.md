# SaMD-Pediatría: Sistema de Soporte a la Decisión Clínica (CDSS) en Infecciones del SNC

[![GitHub Pages](https://img.shields.io/badge/Demo%20en%20Vivo-GitHub%20Pages-0f4c81?style=for-the-badge&logo=github)](https://cifu5050-sketch.github.io/Samd-pediatria/)
[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/Machine%20Learning-Random%20Forest-F7931E?style=for-the-badge&logo=scikit-learn)](https://scikit-learn.org/)

> **Acceso directo a la herramienta clínica interactiva (CDSS):**  
> 👉 **[https://cifu5050-sketch.github.io/Samd-pediatria/](https://cifu5050-sketch.github.io/Samd-pediatria/)**

---

## 📋 Resumen del Proyecto

**Título:** *Desarrollo y validación retrospectiva de un Software as a Medical Device (SaMD) para la optimización diagnóstica y el desescalamiento antimicrobiano en infecciones del sistema nervioso central en pediatría.*

En pediatría, más de la mitad de los pacientes con sospecha de meningitis o encefalitis carecen de un aislamiento microbiológico convencional oportuno, lo que prolonga la terapia antimicrobiana empírica (DOT), el uso innecesario de aciclovir y la estancia hospitalaria. Aunque el panel molecular multiplex **FilmArray Meningitis/Encefalitis (ME)** acelera el diagnóstico etiológico, la presencia de pleocitosis en el líquido cefalorraquídeo (LCR) sigue generando incertidumbre clínica.

Este proyecto integra de forma sistemática:
1. **Resultado del Panel Molecular FilmArray ME** (14 blancos diagnósticos + detección de policoinfecciones).
2. **Cálculo automatizado del *Bacterial Meningitis Score* (BMS)** (escala paraclínica validada de 0 a 4 puntos).
3. **Biomarcadores inflamatorios séricos**, específicamente la **Proteína C Reactiva (PCR)** con punto de corte de **$0.82\text{ mg/dL}$** y la ventana de evolución febril (**$12\text{ horas}$**).

---

## 🏗️ Arquitectura del Software

El ecosistema se divide en dos módulos complementarios desarrollados como parte de la investigación doctoral:

### 1. Módulo de Auditoría Retrospectiva y Aprendizaje Automático (`samd_revisor_hc_2.py`)
* **Procesamiento de Historias Clínicas:** Estandarización automática de bases de datos clínicas (`.xlsx`, `.csv`, `.txt`).
* **Motor Determinístico de Fenotipos:** Clasificación de cada paciente según los flujogramas clínicos del estudio, garantizando un **criterio de seguridad del 100%** en meningitis bacteriana confirmada.
* **Análisis Estadístico de *Antimicrobial Stewardship*:** Comparación pareada (pruebas de Wilcoxon y *t*-Student) entre los días reales de terapia antimicrobiana (**DOT**), uso de **Aciclovir** y **Estancia Hospitalaria (LOS)** frente a la recomendación del SaMD.
* **Modelo Predictivo (*Random Forest Classifier*):** Implementado en `scikit-learn` eliminando la colinealidad entre variables compuestas (utilizando el `BMS_SCORE` consolidado junto con `LEUCOCITOS LCR`, `PANEL_FILMARRAY_ME` y `PCR`).

### 2. Interfaz Web de Soporte a la Decisión Clínica (`index.html`)
* **Calculadora Clínica en Tiempo Real:** Evalúa los 14 patógenos del panel FilmArray ME (6 bacterias, 6 virus y 2 levaduras), policoinfecciones ($\ge 2$ microorganismos) y paneles negativos con pleocitosis (*zona gris*).
* **Desglose Paraclínico del BMS (0 a 4 puntos):**
  1. Tinción de Gram en LCR positiva ($+1\text{ pt}$)
  2. Conteo de neutrófilos en LCR $\ge 1.000\text{ cél/}\mu\text{L}$ ($+1\text{ pt}$)
  3. Proteínas en LCR $\ge 80\text{ mg/dL}$ ($+1\text{ pt}$)
  4. Conteo de neutrófilos en sangre periférica $\ge 10.000\text{ cél/}\mu\text{L}$ ($+1\text{ pt}$)
* **Exportación Clínica:** Generación de reporte imprimible en PDF (`Cmd + P`) y descarga directa de nota de evolución clínica en texto plano (`.txt`).
* **Telemetría de Accesos:** Contador híbrido que discrimina sesiones de profesionales de la salud frente a rastreadores y agentes de Inteligencia Artificial.

---

## 📚 Referencias Científicas y Regulatorias

1. **GBD 2023 Meningitis Collaborators.** Global, regional, and national burden of meningitis, its risk factors, and aetiologies, 1990–2023: a systematic analysis for the Global Burden of Disease Study 2023. *Lancet Neurol*. 2026;25(5):451-468. [doi:10.1016/S1474-4422(26)00101-8](https://doi.org/10.1016/S1474-4422(26)00101-8)
2. **Trujillo-Gomez J, Tsokani S, Arango-Ferreira C, et al.** Biofire FilmArray Meningitis/Encephalitis panel for the aetiological diagnosis of central nervous system infections: a systematic review and diagnostic test accuracy meta-analysis. *EClinicalMedicine*. 2022;44:101275. [doi:10.1016/j.eclinm.2022.101275](https://doi.org/10.1016/j.eclinm.2022.101275)
3. **Hagen A, Eichinger A, Meyer-Buehn M, Schober T, Huebner J.** Comparison of antibiotic and acyclovir usage before and after the implementation of an on-site FilmArray meningitis/encephalitis panel in an academic tertiary pediatric hospital. *BMC Pediatr*. 2020;20(1):56. [doi:10.1186/s12887-020-1944-2](https://doi.org/10.1186/s12887-020-1944-2)
4. **Churcher LM, Bartlett AW, Pendle S, Slade R, Williams PCM.** Impact of the BioFire FilmArray Meningitis-Encephalitis Panel on management of suspected paediatric central nervous system infections. *J Paediatr Child Health*. 2026;62(4):586-593. [doi:10.1111/jpc.70303](https://doi.org/10.1111/jpc.70303)
5. **Messacar K, Parker SK, Todd JK, Dominguez SR.** Implementation of rapid molecular infectious disease diagnostics: the role of diagnostic and antimicrobial stewardship. *J Clin Microbiol*. 2017;55(3):715-723. [doi:10.1128/JCM.02264-16](https://doi.org/10.1128/JCM.02264-16)
6. **Nigrovic LE, Kuppermann N, Macias CG, et al.** Clinical prediction rule for identifying children with cerebrospinal fluid pleocytosis at very low risk of bacterial meningitis. *JAMA*. 2007;297(1):52-60. [doi:10.1001/jama.297.1.52](https://doi.org/10.1001/jama.297.1.52)
7. **Mintegi S, García S, Martín MJ, et al.** Clinical prediction rule for distinguishing bacterial from aseptic meningitis. *Pediatrics*. 2020;146(3):e20201126. [doi:10.1542/peds.2020-1126](https://doi.org/10.1542/peds.2020-1126)
8. **IMDRF SaMD Working Group.** *Software as a Medical Device (SaMD): Clinical Evaluation*. IMDRF/SaMD WG/N41FINAL:2017. International Medical Device Regulators Forum; 2017.

---

## ⚖️ Aviso Legal y Regulatorio (Normativa Colombiana — SaMD / CDSS)

> *"Esta herramienta de soporte a la decisión clínica (CDSS) se basa en la validación retrospectiva de marcadores en LCR. No reemplaza el juicio clínico del profesional tratante, quien asume la responsabilidad final de la terapia instaurada y el manejo del paciente."*

---

## 👨‍💻 Autoría y Créditos

* **Investigador Principal:** Andrés Cifuentes — Proyecto Doctoral en Programación / Ciencias Biomédicas (Antioquia, Colombia, 2026).