# Armonizador de Datos de Salud

**Estandarización, y consolidación de información de distintas fuentes**  
*Mapeo a 31 variables · Unificación de planillas Excel · Normalización de datos · Histórico persistente*

Aplicación web profesional diseñada para procesar, homologar y consolidar datos clínicos, obstétricos y demográficos de pacientes a partir de planillas y archivos Excel (.xlsx, .xls) o CSV con esquemas heterogéneos.

---

## 📑 Análisis Multi-Hoja con Consolidación Selectiva (Consolidar Restantes vs. Separar sin Eliminar)

Para archivos que contienen múltiples hojas de cálculo (o combinación de libros):
1. **Analizar cada hoja individualmente:** Muestra el número de personas, columnas y mapeo de variables de cada hoja por separado.
2. **Eliminar las que no necesites (`🗑️ Eliminar hoja`):** Puedes descartar hojas innecesarias en el **Paso 1** (selector de archivos) o en caliente durante el **Paso 2** (mapeo de variables) con el botón destacado `🗑️ Eliminar esta hoja`.
3. **Diferenciar hojas para separar sin eliminar (`📂 Separar`):** Puedes marcar con un solo clic cualquier hoja que desees mantener de forma independiente (ej: personal de guardia, estadísticas o servicios especiales). La hoja se estandariza a las 31 variables pero **no se mezcla** con el resto.
4. **Conformar un solo consolidado con las restantes (`📦 Consolidar`):** Todas las hojas restantes marcadas para consolidar se fusionan en una **única lista maestra**, con deduplicación por DNI, trazabilidad de fuentes y resolución interactiva de discrepancias.
5. **Panel de Distribución en Paso 3:** Permite revisar y alternar con un clic el destino de cada hoja antes de procesar el resultado final.
6. **Descarga Híbrida Completa:**  
   - **`📥 Descargar Excel Completo (.xlsx)`**: Crea un único libro Excel con la pestaña **`Consolidado_Principal`** al inicio, seguido de cada una de las hojas separadas en sus propias pestañas.
   - Botones para descargar por separado sólo el consolidado o sólo las hojas independientes.
   - Selector de pestañas en la previsualización del Paso 4 para examinar cualquier hoja o el consolidado.

---

## 📝 Opción de Texto Libre para Otras Observaciones (Validación)

En el paso de resolución de discrepancias:
- **Reemplazo de la opción de fallecido por texto libre:** Cada tarjeta de paciente en validación incluye un campo de **`📝 Otras observaciones (Texto libre)`**.
- **Notas manuales directas:** Permite al operador ingresar cualquier anotación clínica, directiva o seguimiento personalizado (ej: *«Requiere seguimiento en nefrología ambulatoria»*, *«Traslado programado»*).
- **Integración automática:** Se combina limpiamente en la columna **`Observaciones`** del consolidado con el distintivo **`[Observación manual]: ...`** y se destaca visualmente en la tabla final con una etiqueta **`📝 Manual`**.

---

## 📞 Teléfono, Teléfono 2 y Teléfono 3 (Auto-Asignación sin Validación)

Para evitar interrupciones innecesarias al operador:
- **Auto-distribución secuencial:** Si una persona con el mismo DNI tiene **números de teléfono diferentes en distintas planillas o bases**, el sistema **no pide validación**. En su lugar, asigna automáticamente el primer número a **`Teléfono`**, el segundo número a **`Teléfono 2`** y el tercero a **`Teléfono 3`**.
- **Mapeo directo:** Cada hoja o planilla también puede mapear directamente sus columnas secundarias de contacto (`Celular`, `Tel_Alternativo`, `Contacto_Emergencia`) a `Teléfono 2` o `Teléfono 3`.

---

## 🏠 Domicilio de RENAPER y Domicilio

Diferenciación de domicilios:
- **`Domicilio`:** Domicilio declarado o de atención informado en el establecimiento o centro de salud.
- **`Domicilio de RENAPER`:** Domicilio legal registrado en el Registro Nacional de las Personas (RENAPER) o padrón electoral, fundamental para cruces territoriales y asignación de efectores.

---

## 🫁 Soporte Respiratorio: Electrodependencia, Oxígeno Dependiente y VIN

En el bloque de datos de salud:
1. **Electrodependencia** *(si, no)*
2. **Oxigeno dependiente** *(si, no)*
3. **VIN** *(si, no - Ventilación Invasiva / No Invasiva / Asistencia Ventilatoria Mecánica)*: Ubicada **a continuación inmediata de Oxigeno dependiente**, permitiendo identificar pacientes con soporte respiratorio avanzado.

---

## 🎂 Cálculo Automático de Edad

Se incorporó la variable **Edad** dentro del bloque de **Datos personales**:
- **Cálculo automático:** Se calcula en tiempo real comparando la **Fecha de nacimiento** del paciente contra la **fecha actual** (años cumplidos en número entero).
- **Formatos admitidos:** Reconoce fechas en formato argentino (`DD/MM/AAAA`), formato ISO (`AAAA-MM-DD`), fechas nativas y números de serie de Excel.
- **Flexibilidad en el mapeo:** Permite utilizar el cálculo automático predeterminado (`⚡ Auto-calcular desde Fecha de nacimiento`) o asignar una columna de edad existente en el archivo original si ya venía informada.

---

## 🤰 Variables Obstétricas y Cálculo Automático de EG

Seguimiento clínico de personas gestantes:
1. **Embarazo**: `(si, no)`
2. **FUM**: Fecha de Última Menstruación *(admite `DD/MM/AAAA`, `AAAA-MM-DD`, etc.)*.
3. **EG (Edad Gestacional)**: **Calculada automáticamente** a partir de la FUM:
   - **Fórmula:** `(Fecha de hoy - FUM) / 7` en semanas completas (número entero: `Math.floor(días / 7)`).
   - **Regla mayor de 41:** Si el cálculo resulta en más de 41 semanas, asigna automáticamente la palabra **`"nacido"`**.

---

## 🔗 Detección y Fusión de DNI Duplicados (Múltiples Fuentes)

Cuando una persona (por su **DNI**) figura en **dos o más hojas o archivos diferentes**:
- **Sin duplicar:** El consolidado final conserva **un único registro por persona**.
- **Ambas fuentes indicadas:** En la columna `Archivo_Origen` se listan todas las procedencias separadas por pleca:  
  *Ejemplo:* `hospital_tres_salas.xlsx [Hoja: Sala_General] | hospital_tres_salas.xlsx [Hoja: Consultorios_Externos]`.
- **Enriquecimiento de datos:** Si la primera hoja tenía algún dato vacío (ej. Teléfono o Domicilio) y la segunda hoja sí lo contiene, se autocompleta sin pisar los datos existentes.
- **Observaciones unificadas:** Si existen notas distintas en cada planilla, se combinan con la referencia de procedencia.
- **Distintivo visual:** En la tabla de previsualización se destaca con una etiqueta **`🔗 2+ fuentes`**.

---

## ⚖️ Paso de Validación: Resolución de Discrepancias entre Fuentes

Si al consolidar distintas bases o planillas una variable contiene **dos o más respuestas diferentes para un mismo DNI** (por ejemplo, *Obra Social: IOMA* en la Sala General vs *Obra Social: PAMI* en Consultorios Externos):

1. **Detección Automática de Conflictos:**  
   El sistema compara campo por campo para cada persona coincidente. Si ambas fuentes informan un dato no vacío pero con valores divergentes, se detiene antes de la descarga y activa el **Paso de Validación de Discrepancias**.
2. **Interfaz Interactiva de Decisión:**  
   - **Agrupado por Paciente:** Muestra el nombre, DNI y las fuentes de origen en conflicto.
   - **Tarjetas Seleccionables:** Para cada variable conflictiva se presentan las respuestas registradas en cada planilla con su fuente de origen para elegir cuál debe prevalecer con un solo clic.
   - **Opción de Valor Manual Personalizado:** Permite escribir un valor corregido si ninguna de las opciones originales es la adecuada.
3. **Botones de Selección Rápida en Lote:**  
   Permite aplicar con un clic todas las respuestas de una fuente determinada (ej. *"⚡ Elegir todas de: Sala_General"*), ideal para planillas masivas.
4. **Buscador y Filtro:**  
   Filtro en tiempo real por DNI, nombre de paciente o variable en conflicto.
5. **Trazabilidad y Auditoría en la Tabla Final:**  
   Las celdas donde el operador resolvió una discrepancia se identifican en la tabla final con una etiqueta destacada **`⚖️ Validado`**, garantizando transparencia en la homologación sanitaria.
6. **Retorno y Edición Flexible:**  
   Desde la pantalla de descarga (Paso 4) se puede reabrir la validación en cualquier momento mediante el botón **`⚖️ Revisar Validación`** para modificar cualquier decisión tomada.

---

## 🔄 Repetición y Retorno Libre entre Pasos

La aplicación cuenta con una barra de navegación interactiva y controles en cada pantalla que te permiten **volver atrás o repetir cualquier paso en cualquier momento**:

- **Barra de Pasos Interactiva (Stepper):**  
  Puedes hacer clic directamente en **Paso 1**, **Paso 2**, **Paso 3** o **Paso 4** para desplazarte libremente entre la subida de archivos, el mapeo de variables, la elección de consolidación y la descarga.
- **Botones de retorno específicos en cada pantalla:**  
  Tanto en el Paso 4 como en el Paso 3 encontrarás botones directos para volver a modificar mapeos, cambiar ordenamientos o agregar más hojas sin perder lo trabajado.
- **Repetición de pasos con Documentos del Histórico:**  
  Al ingresar a **🕒 Histórico de Documentos**, además de poder unir planillas directamente a descarga, dispones de:
  - **`✏️ Cargar y Repetir Pasos (Paso 2)`**: Carga los documentos seleccionados en la pantalla de mapeo para que puedas revisar las variables, cambiar columnas asignadas, reordenar y consolidar de nuevo.
  - **`🔄 Re-abrir y editar pasos`**: Botón individual en cada documento histórico para retomar su configuración desde el inicio.

---

## 📋 Estructura de Variables del Sistema (31 Variables Estándar)

El sistema mapea y genera una planilla maestra de **31 variables estándar** (más la columna de trazabilidad `Archivo_Origen` en el consolidado):

### 1. Datos personales (12 variables)
1. **Apellido y nombre**
2. **DNI**
3. **Sexo**
4. **Fecha de nacimiento**
5. **Edad** *(Calculada automáticamente en años cumplidos: Fecha actual - Fecha de nacimiento)*
6. **Domicilio**
7. **Domicilio de RENAPER** *(Domicilio legal / padrón electoral)*
8. **Localidad**
9. **Provincia**
10. **Teléfono** *(Contacto principal)*
11. **Teléfono 2** *(Segundo contacto / auto-asignado si hay teléfonos distintos)*
12. **Teléfono 3** *(Tercer contacto o de emergencia / auto-asignado)*

### 2. Prestación de salud (3 variables)
13. **Efector** *(Unificación de Hospital, Centro de salud, CAPS, sala o establecimiento asistencial)*
14. **Centro de diálisis**
15. **OBRA SOCIAL** *(PAMI, IOMA, prepagas, obras sociales)*

### 3. Datos de Salud, Embarazo y Observaciones (16 variables)
16. **Diagnóstico** *(texto)*
17. **Movilidad** *(si, postrado, silla de ruedas, con dificultad)*
18. **I.RENAL** *(si, no)*
19. **Diálisis** *(HD, Peritoneal, no)*
20. **PULMONAR** *(si, no / EPOC / afección respiratoria)*
21. **Electrodependencia** *(si, no)*
22. **Oxigeno dependiente** *(si, no)*
23. **VIN** *(si, no - Ventilación Invasiva / No Invasiva / Soporte ventilatorio, a continuación de Oxigeno dependiente)*
24. **Obesidad** *(si, no)*
25. **DBT** *(ID, IR, no)*
26. **NEUROLOGICO** *(si, no)*
27. **SM** *(si, no - Salud Mental)*
28. **Embarazo** *(si, no)*
29. **FUM** *(Fecha de Última Menstruación: DD/MM/AAAA o AAAA-MM-DD)*
30. **EG** *(Edad Gestacional calculada en semanas enteras; si > 41 coloca "nacido")*
31. **Observaciones** *(texto libre / notas adicionales / notas manuales de validación)*

### 4. Trazabilidad y Origen
- **Archivo_Origen** *(Nombre del archivo y hoja de procedencia; indica fuentes múltiples si el DNI coincide en más de una planilla)*

---

## 🚀 Acceso a la Aplicación

- **En el navegador:** Abre directamente [**`index.html`**](file:///C:/Users/nerin/.gemini/antigravity/scratch/proyecto_excel/index.html) o entra a [**http://localhost:3000**](http://localhost:3000).
- **Archivos de prueba incluidos:**
  - `hospital_tres_salas.xlsx` (generable con 1 clic desde el botón *"📑 Probar Excel con 3 Hojas (Multi-Hojas)"* en la app).
  - [`ejemplo_hospital_san_martin.csv`](file:///C:/Users/nerin/.gemini/antigravity/scratch/proyecto_excel/ejemplo_hospital_san_martin.csv)
  - [`ejemplo_centro_salud_norte.csv`](file:///C:/Users/nerin/.gemini/antigravity/scratch/proyecto_excel/ejemplo_centro_salud_norte.csv)
