# Práctica DFIR — Análisis Forense Digital y Respuesta a Incidentes

Memoria técnica de la práctica del módulo de **Digital Forensics & Incident Response (DFIR)** del Bootcamp de Ciberseguridad Full Stack (CS11) de KeepCoding.

El trabajo cubre tres áreas del análisis forense: investigación de un incidente en un disco Windows, adquisición y análisis de memoria RAM, y estudio de metadatos en imágenes enviadas por distintas plataformas.

---

## Contenido del repositorio

| Archivo | Descripción |
|---------|-------------|
| `Memoria_DFIR_Nico.docx` | Informe completo con capturas, explicaciones paso a paso y tabla resumen de resultados |
| `README.md` | Este fichero |

---

## Qué se hizo

### 1. Análisis forense de Windows (CTF)

A partir de una imagen de disco (`Win10_PC001.vmdk`), se investigó un incidente de seguridad reconstruyendo las acciones del atacante. Se completaron los 28 retos del CTF de la plataforma ctf.ciberforensic.es, incluyendo:

- Identificación del equipo comprometido a través del registro de Windows (hive SYSTEM)
- Localización de herramientas de ataque (nbtscan, xCmd, WMIBackdoor.ps1) y software de control remoto (TeamViewer)
- Recuperación de ficheros eliminados desde la papelera ($Recycle.Bin)
- Extracción y crackeo de credenciales a partir de los hives SAM y SYSTEM con Mimikatz
- Identificación de conexiones del atacante (IP, puerto, ID de TeamViewer) mediante el análisis de logs de seguridad (Security.evtx) y logs de TeamViewer
- Análisis de artefactos de ejecución (Prefetch) para establecer la línea temporal del ataque
- Cálculo y verificación del hash SHA-256 de la evidencia

### 2. Adquisición y análisis de memoria RAM

- Volcado de memoria de un sistema Windows con **winpmem**
- Análisis del dump con **Volatility 3**: listado de procesos (`windows.pslist`) y conexiones de red (`windows.netscan`)
- CTF extra sobre una imagen de RAM comprometida (`20230810.mem`):
  - Detección de un proceso malicioso (`svchost.exe` lanzado desde `explorer.exe`) mediante `windows.pstree`
  - Identificación de la conexión C2 (IP + puerto) con `windows.netscan`
  - Confirmación de código inyectado con `windows.malfind`
  - Extracción del binario malicioso con `windows.filescan` + `windows.dumpfiles` y cálculo de su hash MD5

### 3. Comparativa de metadatos

Experimento sobre cómo las plataformas de mensajería modifican los metadatos EXIF de una fotografía al enviarla. Se analizaron con **exiftool** las versiones recibidas por:

- WhatsApp — elimina todos los metadatos, recomprime la imagen
- Telegram — elimina EXIF/GPS, conserva un perfil ICC propio (sRGB/Qt)
- Email (Gmail) — conserva la imagen y todos sus metadatos intactos
- Signal — elimina EXIF/GPS, añade un perfil ICC de Google

---

## Herramientas utilizadas

| Herramienta | Uso |
|-------------|-----|
| FTK Imager | Navegación y exportación de artefactos de la imagen de disco |
| Registry Explorer | Análisis de hives del registro de Windows |
| PECmd | Análisis de artefactos Prefetch (ejecución de programas) |
| EvtxECmd | Conversión de logs de eventos (.evtx) a CSV |
| Timeline Explorer | Visualización y filtrado de eventos de seguridad |
| Hasher | Cálculo de hashes (SHA-256) |
| Mimikatz | Extracción de hashes NTLM desde SAM/SYSTEM |
| winpmem | Adquisición de memoria RAM |
| Volatility 3 | Análisis forense de volcados de memoria |
| exiftool | Lectura y comparación de metadatos EXIF |

---

## Entorno

- **VM forense:** Windows10_forensic_alumnos (VirtualBox)
- **Host:** Mac Intel (macOS)
- **Evidencia de disco:** Win10_PC001.vmdk (montada como disco SATA secundario, acceso en modo lectura desde FTK Imager)
- **Evidencia de RAM:** 20230810.mem (imagen proporcionada para el CTF extra)

---

## Autor

**Nico** — Bootcamp Ciberseguridad Full Stack CS11, KeepCoding (2026)
