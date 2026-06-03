# MR2023 — Experimentos con Dron Tello para Modelación Dinámica

Repositorio base para la **Sesión 6 — Campaña experimental de dinámica vertical del dron** del curso **MR2023 Modelación para Automatización**.

El objetivo de estos proyectos es que los estudiantes adquieran datos reales de altura de un dron tipo **DJI Tello**, generen archivos CSV, visualicen la respuesta temporal y preparen evidencia experimental para identificar modelos dinámicos preliminares de primer y segundo orden.

La variable central del laboratorio es la **altura**:

\[
h(t)
\]

La actividad conecta el flujo profesional de modelación:

\[
\text{Vuelo} \rightarrow \text{Datos} \rightarrow \text{Gráfica} \rightarrow \text{Parámetros} \rightarrow \text{Modelo} \rightarrow \text{Validación}
\]

---

## 1. Propósito académico

Estos códigos apoyan una experiencia experimental donde el estudiante aprende a:

- Ejecutar pruebas seguras con un dron Tello.
- Registrar tiempo, altura y referencia experimental.
- Generar datasets en formato CSV.
- Visualizar altura contra tiempo.
- Identificar parámetros preliminares como:
  - ganancia \(K\),
  - constante de tiempo \(\tau\),
  - sobreimpulso aproximado,
  - error RMS.
- Comparar datos reales contra modelos dinámicos simulados en MATLAB/Simulink.
- Discutir la validez y límites de un modelo obtenido con datos experimentales reales.

El laboratorio no busca que el dron “vuele perfecto”, sino que los estudiantes aprendan a convertir una medición física imperfecta en evidencia técnica defendible.

---

## 2. Proyectos incluidos

Este repositorio puede organizarse con los siguientes paquetes ZIP.

| Archivo ZIP | Proyecto | Propósito |
|---|---|---|
| `MR2023_Tello_HelloWorld.zip` | Hello World Tello | Puesta a punto inicial: conexión, lectura de batería/telemetría y primeras gráficas. |
| `sesion06_bloque2_python_v3.zip` | Experimento patrón común | Escalón relativo de altura desde hover estable. Dataset base de la sesión. |
| `sesion06_equipo01_repetibilidad_audited.zip` | Equipo 1 — Repetibilidad experimental | Tres repeticiones del mismo escalón para evaluar variabilidad. |
| `sesion06_equipo02_amplitud_audited.zip` | Equipo 2 — Efecto de amplitud | Comparación de respuestas ante escalones de diferente magnitud. |
| `sesion06_equipo03_descenso_audited.zip` | Equipo 3 — Escalón descendente | Análisis de la dinámica al bajar. |
| `sesion06_equipo04_secuencia_audited.zip` | Equipo 4 — Secuencia de escalones | Respuesta ante dos cambios consecutivos de referencia. |
| `sesion06_equipo05_punto_operacion_audited.zip` | Equipo 5 — Punto de operación | Comparación del mismo escalón en una zona baja y una zona alta. |
| `sesion06_equipo06_ascenso_descenso_audited.zip` | Equipo 6 — Ascenso-descenso | Ascenso seguido de descenso en un mismo vuelo. |

---

## 3. Requerimientos

### Hardware recomendado

- Dron **DJI Tello** o compatible con `djitellopy`.
- Computadora con Wi-Fi.
- Zona despejada de vuelo interior.
- Batería del dron preferentemente mayor a **70 %** para pruebas de clase.
- Altura máxima recomendada: **1.5 m**.

### Software

- Python 3.10 o superior.
- Visual Studio Code o editor equivalente.
- Paquetes Python:

```txt
djitellopy>=2.5.0
pandas>=2.0
matplotlib>=3.8
numpy>=1.26
```

Algunos paquetes pueden incluir versiones ligeramente distintas en su `requirements.txt`; se recomienda usar el archivo propio de cada proyecto.

---

## 4. Instalación general

Después de descargar y descomprimir uno de los proyectos:

```bash
cd nombre_del_proyecto
python -m venv .venv
```

En Windows:

```bash
.venv\Scripts\activate
pip install -r requirements.txt
```

En macOS/Linux:

```bash
source .venv/bin/activate
pip install -r requirements.txt
```

---

## 5. Flujo recomendado antes de volar

Antes de ejecutar cualquier prueba real:

1. Descomprimir el proyecto correspondiente.
2. Crear y activar el ambiente virtual.
3. Instalar dependencias.
4. Ejecutar primero en modo simulación `--mock`.
5. Revisar que se generen archivos en `datos/` y `salidas/`.
6. Encender el Tello.
7. Conectarse al Wi-Fi del Tello.
8. Confirmar zona despejada.
9. Confirmar batería suficiente.
10. Ejecutar el script real únicamente con autorización del profesor.

Ejemplo:

```bash
python src/experimento_patron_tello_v3.py --mock
python src/experimento_patron_tello_v3.py
```

---

## 6. Proyecto 0 — Hello World Tello

**Archivo:** `MR2023_Tello_HelloWorld.zip`

Este proyecto se usa antes de la campaña experimental para verificar que el entorno funciona.

### Estructura esperada

```txt
MR2023_Tello_HelloWorld/
├── requirements.txt
├── data/
├── figures/
└── src/
    ├── hello_tello.py
    ├── hello_tello_takeoff.py
    └── plot_hello_tello.py
```

### Propósito

- Verificar conexión con el Tello.
- Leer datos básicos de telemetría.
- Generar un CSV simple.
- Graficar datos iniciales.
- Reducir fricción técnica antes de la sesión experimental.

### Ejecución sugerida

```bash
python src/hello_tello.py
python src/plot_hello_tello.py
```

El archivo `hello_tello_takeoff.py` debe utilizarse únicamente cuando la zona de vuelo esté autorizada.

---

## 7. Proyecto patrón — Bloque 2

**Archivo:** `sesion06_bloque2_python_v3.zip`

Este proyecto implementa el **experimento patrón común** de la sesión.

### Estructura esperada

```txt
sesion06_bloque2_python_v3/
├── requirements.txt
├── datos/
├── salidas/
└── src/
    ├── experimento_patron_tello_v3.py
    └── validar_csv_patron_v3.py
```

### Experimento

En lugar de forzar una altura absoluta exacta, el código mide la altura real después del despegue y aplica un escalón relativo:

\[
h_{0,real} \rightarrow h_{0,real}+30\text{ cm}
\]

### Ejecución

Modo simulación:

```bash
python src/experimento_patron_tello_v3.py --mock
```

Modo real:

```bash
python src/experimento_patron_tello_v3.py
```

Validación del CSV:

```bash
python src/validar_csv_patron_v3.py --csv datos/experimento_patron_escalon_030cm.csv
```

### Salidas principales

```txt
datos/experimento_patron_escalon_030cm.csv
datos/experimento_patron_escalon_030cm_metadata.json
salidas/experimento_patron_escalon_030cm.png
```

---

## 8. Equipo 1 — Repetibilidad experimental

**Archivo:** `sesion06_equipo01_repetibilidad_audited.zip`

### Pregunta experimental

> ¿El dron responde igual ante la misma entrada experimental?

### Experimento

Tres repeticiones del mismo escalón:

\[
h_{0,real} \rightarrow h_{0,real}+30\text{ cm}
\]

### Script principal

```bash
python src/experimento_repetibilidad_equipo01.py
```

Modo simulación:

```bash
python src/experimento_repetibilidad_equipo01.py --mock
```

### Salidas esperadas

```txt
datos/equipo01_repetibilidad_030cm_rep01.csv
datos/equipo01_repetibilidad_030cm_rep02.csv
datos/equipo01_repetibilidad_030cm_rep03.csv
datos/equipo01_repetibilidad_resumen.csv
salidas/equipo01_repetibilidad_comparacion.png
```

### Análisis esperado

- Comparar las tres curvas.
- Estimar variabilidad en \(K\).
- Estimar variabilidad en \(\tau\).
- Identificar la repetición más adecuada para modelación.

---

## 9. Equipo 2 — Efecto de amplitud

**Archivo:** `sesion06_equipo02_amplitud_audited.zip`

### Pregunta experimental

> ¿Un cambio mayor modifica la dinámica observada?

### Experimento

Comparación de dos escalones relativos:

\[
h_{0,real} \rightarrow h_{0,real}+30\text{ cm}
\]

\[
h_{0,real} \rightarrow h_{0,real}+40\text{ cm}
\]

La decisión de usar 30 cm y 40 cm busca mejorar la calidad de medición frente a la resolución aproximada de altura del Tello, que puede estar alrededor de 10 cm.

### Script principal

```bash
python src/experimento_amplitud_equipo02.py
```

Modo simulación:

```bash
python src/experimento_amplitud_equipo02.py --mock
```

### Salidas esperadas

```txt
datos/equipo02_amplitud_030cm.csv
datos/equipo02_amplitud_040cm.csv
datos/equipo02_amplitud_resumen.csv
salidas/equipo02_amplitud_comparacion.png
```

### Análisis esperado

- Comparar rapidez de respuesta.
- Comparar sobreimpulso.
- Comparar ganancia \(K\).
- Evaluar si la amplitud afecta la validez de un modelo de primer orden.

---

## 10. Equipo 3 — Escalón descendente

**Archivo:** `sesion06_equipo03_descenso_audited.zip`

### Pregunta experimental

> ¿Bajar es dinámicamente equivalente a subir?

### Experimento

El dron despega, se estabiliza, sube previamente a una zona segura y luego aplica un descenso relativo:

\[
h_{alto,real} \rightarrow h_{alto,real}-30\text{ cm}
\]

### Script principal

```bash
python src/experimento_descenso_equipo03.py
```

Modo simulación:

```bash
python src/experimento_descenso_equipo03.py --mock
```

### Salidas esperadas

```txt
datos/equipo03_descenso_030cm.csv
datos/equipo03_descenso_030cm_metadata.json
datos/equipo03_descenso_resumen.csv
salidas/equipo03_descenso_030cm.png
salidas/equipo03_descenso_comparacion.png
```

### Análisis esperado

- Estimar \(K\) y \(\tau\) en descenso.
- Observar posible sobrepaso hacia abajo.
- Comparar la dinámica de descenso contra el experimento patrón de ascenso.

---

## 11. Equipo 4 — Secuencia de escalones

**Archivo:** `sesion06_equipo04_secuencia_audited.zip`

### Pregunta experimental

> ¿Cómo responde el dron ante cambios consecutivos de referencia?

### Experimento

Dos escalones relativos ascendentes en un mismo vuelo:

\[
h_{0,real} \rightarrow h_{0,real}+30\text{ cm} \rightarrow h_{0,real}+50\text{ cm}
\]

Internamente corresponde a:

```txt
move_up(30)
move_up(20)
```

### Script principal

```bash
python src/experimento_secuencia_equipo04.py
```

Modo simulación:

```bash
python src/experimento_secuencia_equipo04.py --mock
```

### Salidas esperadas

```txt
datos/equipo04_secuencia_escalones.csv
datos/equipo04_secuencia_escalones_metadata.json
datos/equipo04_secuencia_escalones_resumen.csv
salidas/equipo04_secuencia_escalones.png
salidas/equipo04_secuencia_escalones_tramos.png
```

### Análisis esperado

- Separar primer y segundo tramo.
- Evaluar si el sistema alcanza régimen antes del segundo cambio.
- Comparar parámetros por tramo.
- Discutir si un único modelo describe toda la secuencia.

---

## 12. Equipo 5 — Punto de operación

**Archivo:** `sesion06_equipo05_punto_operacion_audited.zip`

### Pregunta experimental

> ¿El mismo cambio de altura se comporta igual en diferentes zonas de operación?

### Experimento

Se realizan dos pruebas separadas:

Zona baja:

\[
h_{bajo,real} \rightarrow h_{bajo,real}+30\text{ cm}
\]

Zona alta:

\[
h_{alto,real} \rightarrow h_{alto,real}+30\text{ cm}
\]

La zona alta se obtiene con una subida previa controlada antes del escalón de identificación.

### Script principal

```bash
python src/experimento_punto_operacion_equipo05.py
```

Modo simulación:

```bash
python src/experimento_punto_operacion_equipo05.py --mock
```

### Salidas esperadas

```txt
datos/equipo05_punto_operacion_bajo_030cm.csv
datos/equipo05_punto_operacion_alto_030cm.csv
datos/equipo05_punto_operacion_resumen.csv
salidas/equipo05_punto_operacion_comparacion_altura.png
salidas/equipo05_punto_operacion_comparacion_desviacion.png
```

### Análisis esperado

- Comparar \(K\) en zona baja y zona alta.
- Comparar \(\tau\) en zona baja y zona alta.
- Discutir validez local del modelo lineal.
- Analizar posible efecto de la altura inicial sobre la dinámica observada.

---

## 13. Equipo 6 — Ascenso-descenso

**Archivo:** `sesion06_equipo06_ascenso_descenso_audited.zip`

### Pregunta experimental

> ¿Qué ocurre cuando el sistema invierte la dirección de seguimiento de altura?

### Experimento

Un solo vuelo con ascenso y descenso:

\[
h_{0,real} \rightarrow h_{0,real}+30\text{ cm} \rightarrow h_{0,real}
\]

### Script principal

```bash
python src/experimento_ascenso_descenso_equipo06.py
```

Modo simulación:

```bash
python src/experimento_ascenso_descenso_equipo06.py --mock
```

### Salidas esperadas

```txt
datos/equipo06_ascenso_descenso_030cm.csv
datos/equipo06_ascenso_descenso_030cm_metadata.json
datos/equipo06_ascenso_descenso_resumen.csv
salidas/equipo06_ascenso_descenso_030cm.png
salidas/equipo06_ascenso_descenso_tramos.png
salidas/equipo06_ascenso_descenso_desviacion.png
```

### Análisis esperado

- Separar tramo de ascenso y tramo de descenso.
- Comparar \(K_{ascenso}\) contra \(K_{descenso}\).
- Comparar \(\tau_{ascenso}\) contra \(\tau_{descenso}\).
- Discutir si conviene modelar por tramos.

---

## 14. Parámetros recomendados para clase

Salvo autorización del profesor, se recomienda mantener:

```txt
sample_period = 0.10 s
initial_hold = 3 s
after_hold = 6 a 8 s
step_cm = 30 cm
altura máxima = 150 cm
batería mínima absoluta = 35 %
batería recomendada para iniciar = 70 % o más
```

No se recomienda modificar amplitudes durante la sesión, especialmente en equipos con experimentos acumulativos o de zona alta.

---

## 15. Seguridad operacional

Antes de cada vuelo:

- Confirmar zona despejada.
- Confirmar batería suficiente.
- Confirmar conexión al Wi-Fi del Tello.
- Confirmar que nadie esté dentro de la zona de vuelo.
- Confirmar autorización explícita del profesor.
- Mantener a un estudiante como responsable de seguridad.

Durante el vuelo:

- No entrar a la zona de vuelo.
- No acercar manos al dron.
- Aterrizar si hay deriva lateral peligrosa.
- Cancelar si hay pérdida de conexión.
- Cancelar si aparece comportamiento inestable.

Después del vuelo:

- Verificar que el CSV se generó.
- Revisar la gráfica inicial.
- Registrar observaciones experimentales.

---

## 16. Problemas comunes

### `ModuleNotFoundError: No module named 'djitellopy'`

No se instalaron las dependencias o no está activo el ambiente virtual.

Solución:

```bash
.venv\Scripts\activate
pip install -r requirements.txt
```

### No se conecta el Tello

Verificar:

- El dron está encendido.
- La computadora está conectada al Wi-Fi del Tello.
- No hay otra computadora controlando el dron.
- La batería no está críticamente baja.

### Error `No valid imu`

Puede ocurrir por estado interno del dron, golpe previo, mala calibración, iluminación/superficie inadecuada o condición inestable.

Recomendación:

- Apagar y encender el Tello.
- Verificar superficie plana.
- Revisar calibración si el problema persiste.
- No continuar la prueba si el dron muestra comportamiento anómalo.

### No aparece la gráfica

Los scripts auditados usan un backend no interactivo de Matplotlib para guardar PNG en `salidas/`. No es necesario que se abra una ventana gráfica.

---

## 17. Datos que deben entregar los equipos

Cada equipo deberá conservar:

```txt
CSV experimental
archivo metadata JSON
gráfica PNG
resumen CSV, si el proyecto lo genera
bitácora experimental
observaciones de vuelo
```

Para la tarea posterior, el CSV será usado en MATLAB/Simulink para:

1. importar datos,
2. graficar altura contra tiempo,
3. estimar altura inicial y final,
4. calcular \(\Delta h\),
5. calcular \(\Delta r\),
6. estimar \(K\),
7. estimar \(\tau\) por método del 63 %, 
8. construir un modelo de primer orden,
9. simular el modelo,
10. comparar modelo contra datos experimentales,
11. calcular error RMS,
12. interpretar la calidad del modelo.

---

## 18. Uso recomendado del repositorio

Una estructura sugerida del repositorio es:

```txt
MR2023-Tello-Modelacion/
├── README.md
├── zips/
│   ├── MR2023_Tello_HelloWorld.zip
│   ├── sesion06_bloque2_python_v3.zip
│   ├── sesion06_equipo01_repetibilidad_audited.zip
│   ├── sesion06_equipo02_amplitud_audited.zip
│   ├── sesion06_equipo03_descenso_audited.zip
│   ├── sesion06_equipo04_secuencia_audited.zip
│   ├── sesion06_equipo05_punto_operacion_audited.zip
│   └── sesion06_equipo06_ascenso_descenso_audited.zip
└── docs/
    └── instructivos_de_clase/
```

También puede subirse cada proyecto descomprimido en carpetas separadas si se desea que los alumnos inspeccionen el código directamente desde GitHub.

---

## 19. Mensaje para los estudiantes

Este laboratorio reproduce, a escala académica, una práctica real de ingeniería: antes de diseñar controladores sofisticados, se necesita medir, validar y entender la dinámica del sistema.

Un CSV no es todavía un modelo. Una gráfica no es todavía una explicación. Una función de transferencia no es todavía validación.

El reto profesional es construir la cadena completa:

\[
\text{Experimento} \rightarrow \text{Datos} \rightarrow \text{Modelo} \rightarrow \text{Validación} \rightarrow \text{Conclusión técnica}
\]

Ese es el corazón de la modelación para automatización.

---

## 20. Autoría y contexto

Material preparado para el curso **MR2023 Modelación para Automatización**, Ingeniería Mecatrónica.

Uso académico: adquisición experimental de datos de altura con dron Tello, identificación preliminar de modelos dinámicos y validación con MATLAB/Simulink.
