# Discovery Agent

> Convierte conocimiento de negocio crudo en artefactos de producto validables.
> Agente de [Claude Code](https://claude.ai/code) para la asignatura de Ingeniería de Software — Maestría en Software, UPS.

---

## Qué es

**Discovery Agent** estructura el proceso de descubrimiento de producto *antes* de escribir una sola línea de código.

Dado un conjunto de entrevistas en crudo, el agente extrae **personas**, **dolores**, **requisitos**, **user stories** y un **MVP Canvas**; luego convierte los supuestos más riesgosos en **hipótesis falsables** con experimentos baratos para validarlas.

El agente es **genérico**: no está atado a ningún dominio. Cada caso de negocio es un *discovery* independiente bajo `discoveries/`.

---

## Flujo de trabajo

```
Entrevistas en Markdown
         │
         ▼
/discovery:analyze       →  personas.md · requisitos.md · evidence-map.json
         │
         ▼
/discovery:generate-mvp  →  user-stories.md · mvp-canvas.md
         │
         ▼
/discovery:experiments   →  hypotheses.md · experiment-board.json
         │
         ▼
/discovery:report        →  report.html  (dashboard visual autocontenido)
```

---

## Estructura del repositorio

```
discovery-agent/
├── .claude/
│   ├── commands/discovery/     # Cuatro comandos: analyze · generate-mvp · experiments · report
│   ├── hooks/                  # readiness-gate.py · hypothesis-gate.py
│   ├── scripts/                # build-report.py (generador de HTML)
│   └── skills/discovery/       # Estándar de formato y trazabilidad
├── discoveries/
│   ├── _template/              # Plantilla vacía para nuevos casos
│   │   ├── interviews/
│   │   └── outputs/
│   └── citasalud/              # Discovery de ejemplo completo
│       ├── interviews/         # recepcionista.md · doctora.md · paciente.md
│       └── outputs/            # Todos los artefactos generados
└── CLAUDE.md                   # Reglas y flujo de trabajo del agente
```

---

## Requisitos previos

| Herramienta | Versión |
|---|---|
| [Claude Code](https://claude.ai/code) | latest |
| Python | 3.x |

---

## Uso rápido

```bash
# 1. Copia la plantilla para tu nuevo caso
cp -r discoveries/_template discoveries/mi-caso

# 2. Agrega entrevistas en discoveries/mi-caso/interviews/
#    Cada archivo debe tener frontmatter (rol_entrevistado, primera_persona, anonimizada)
```

Luego, dentro de Claude Code:

```
/discovery:analyze discoveries/mi-caso
/discovery:generate-mvp discoveries/mi-caso
/discovery:experiments discoveries/mi-caso
/discovery:report discoveries/mi-caso
```

Los artefactos se generan automáticamente en `discoveries/mi-caso/outputs/`.

---

## Artefactos generados

| Archivo | Comando | Contenido |
|---|---|---|
| `personas.md` | `:analyze` | Personas + stakeholders con diagrama Mermaid |
| `requisitos.md` | `:analyze` | Requisitos funcionales y no funcionales (R-NN) |
| `evidence-map.json` | `:analyze` | Mapa de trazabilidad entre entrevistas, personas y dolores |
| `user-stories.md` | `:generate-mvp` | User stories en formato INVEST con criterios de aceptación |
| `mvp-canvas.md` | `:generate-mvp` | Canvas de 7 bloques con flujo Output → Outcome → Impact |
| `hypotheses.md` | `:experiments` | Test cards ordenadas por nivel de riesgo |
| `experiment-board.json` | `:experiments` | Tablero de hipótesis legible por máquina |
| `report.html` | `:report` | Dashboard visual autocontenido, sin dependencias externas |

---

## Reglas de trabajo

| Regla | Descripción |
|---|---|
| **Cero invención** | Ningún artefacto puede afirmar algo que no esté respaldado por una entrevista real |
| **Trazabilidad** | Cada persona, dolor y requisito cita el archivo fuente (p. ej. `recepcionista.md`) |
| **Primera persona** | Una persona solo existe si hay una entrevista en primera persona de ese rol |
| **Aislamiento** | Los artefactos de un discovery nunca se mezclan con otro |
| **Idioma** | Español, salvo términos técnicos (user story, MVP, stakeholder) |

---

## Gates automáticos

Dos hooks de validación bloquean la escritura cuando la evidencia no es suficiente.

### Gate de readiness

Se activa antes de generar `mvp-canvas.md` o `user-stories.md`. Verifica que:

- Existe `evidence-map.json`
- Hay al menos 2 entrevistas
- Cada persona primaria tiene respaldo de primera mano
- No hay dolores huérfanos (sin fuente de entrevista)

### Gate de hipótesis

Se activa antes de generar `hypotheses.md` o `experiment-board.json`. Verifica que:

- Las hipótesis son falsables: tienen métrica, umbral numérico y regla de decisión
- El umbral contiene un número concreto (no "muchos" ni "la mayoría")
- La regla de decisión contempla el escenario de fallo
- La métrica no es de vanidad (descargas, likes, líneas de código, story points)

---

## Discovery de ejemplo

`discoveries/citasalud/` contiene un discovery completo de un sistema de citas médicas:
tres entrevistas en primera persona (recepcionista, doctora, paciente), nueve requisitos
trazados, seis user stories, cinco hipótesis falsables y un dashboard HTML generado.
Úsalo como referencia o como demostración del flujo completo.

---

## Evidencia de la demo del gate

**Pasos seguidos para la demostración:**

1. Eliminar una entrevista de persona referenciada de primera mano que necesitaba respaldo de entrevista. Para el ejemplo se eliminó representante de familia.
2. Ejecutar el comando /discovery:analyze.
3. Una vez generado los archivos del comando anterior, ejecuté el comando /discovery:generate-mvp.
4. El hook readiness-gate.py bloquea la generación del mvp-canvas debido a la falta de evidencia.
5. Se añadió nuevamente la entrevista borrada.
6. Ejecutar el comando /discovery:analyze.
7. Pasó el hook readiness-gate.py debido a que contaba con la evidencia suficiente.

---

## Reflexion Breve

La práctica del Discovery agent sin duda ahorra un montón de tiempo en el análisis de nuevos requerimientos y proceso ya que nos ayuda a analizar y ver el problema desde otro punto de vista.

Al inicio, mientras conversaba sobre el problema presentado en la inscripción de niños y niñas del Oratorio Vacacional, suponía que iba a ser una simple página de registro, un formulario en un sitio web, conectado a una base de datos limitado a la captura de datos básicos.

Pero al levantar y revisar la información del proceso y en conjunto con el agente, entendí que el problema es enorme, no es solo “digitalizar la inscripción remplazando el Google Forms” sino buscar soluciones para todo el flujo posterior a la inscripción: validar datos, organizar grupos por edades, revisar estados de pagos, verificar números de celular, comunicarse con los representantes, etc. Por ejemplo, las inscripciones antes se trabajaban con siete – ocho formularios simultáneamente, es decir el trabajo de comprobación y el nivel o riesgo de errores era altísimo ya que se trabajaba con 50 a 80 registros por formulario y la comprobación manual se volvía un problema.

Por eso, mi supuesto que era “Digitalizar la inscripción” cambio a mejorar la trazabilidad y organización del proceso de inscripción, el Discovery agent ayudo a evidenciar que el MVP no debía centrarse únicamente en el formulario, sino en el registro, organización, validación y consulta de información para acaparar todo el trabajo manual de la coordinación del Oratorio y se puede evidenciar en el archivo de la práctica.

Lo más complicado de cumplir la regla de “cero invenciones” desde mi punto de vista es buscar soluciones rápidas sin entender el problema, por ejemplo, hay partes del proceso que parecen obvias y se termina añadiendo requerimientos o funcionalidades que no se piden, por ejemplo, un reporte, una pasarela de pagos, etc. Sin embargo, el Discovery agent verifica cada uno de los requisitos y si no están respaldadas en las entrevistas, no se le toma en cuenta, es decir si no se identificaba el dolor real en las entrevistas pasan a ser meras ideas o suposiciones.

La gobernanza ejecutable son las reglas del agente que sirven como un control que, si o si debe cumplir. En la práctica cumplió su rol restringiendo la generación de artefactos libremente, no inventando información, manteniendo la trazabilidad con evidencia, validando que los roles o personas principales tengan entrevistas, bloqueando hipótesis que no sea falsables, es decir que los archivos donde definimos las reglas, comandos, hooks, ayudan a controlar el proceso ante falta de evidencia o hipótesis vagas o no medibles. Por eso al utilizar la IA para generar y procesar documentos más rápido la gobernanza ejecutable nos permite controlar y permitiendo que el trabajo generado por la IA sea más confiable debido a que cada output del agente está alineado a reglas predefinidas.
