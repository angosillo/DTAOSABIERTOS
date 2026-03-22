# Agentes Burnout - Documento de Especificaciones

## 1. Visión General

**Nombre del proyecto:** Agentes Burnout

**Descripción:** Sistema de agentes de inteligencia artificial especializado en evaluación clínica de burnout, depresión y ansiedad. El sistema realiza entrevistas clínicas automatizadas a pacientes a través de un bot de Telegram y genera informes internos (formulación de caso y plan terapéutico) para la profesional clínica.

**Usuaria profesional:** Beatriz (psicóloga/terapeuta).

**Usuarios finales:** Pacientes que acceden al bot de Telegram para realizar una pre-entrevista clínica automatizada antes de su consulta con la profesional.

---

## 2. Arquitectura del Sistema

### 2.1 Agentes

El sistema se compone de **3 agentes independientes y especializados** que se comunican entre sí de forma secuencial:

```
┌─────────────┐      ┌─────────────────┐      ┌──────────────────────┐
│   AGENTE 1  │ ──── │    AGENTE 2     │ ──── │      AGENTE 3        │
│ Entrevistador│      │ Formulador de   │      │  Planificador        │
│             │      │ Caso            │      │  Terapéutico         │
└─────────────┘      └─────────────────┘      └──────────────────────┘
     ▲                      │                         │
     │                      ▼                         ▼
  Paciente            Informe interno           Informe interno
 (Telegram)           para Beatriz              para Beatriz
```

#### Agente 1: Entrevistador

- **Interacción:** Directa con el paciente vía Telegram.
- **Función:** Realizar la entrevista clínica estructurada por bloques temáticos.
- **Estilo conversacional:** Cálido, empático, profesional. Adapta el ritmo al paciente.
- **Entrada:** Respuestas del paciente en lenguaje natural.
- **Salida:** Transcripción estructurada de la entrevista con las respuestas categorizadas por bloques.

#### Agente 2: Formulador de Caso

- **Interacción:** Solo interna (sin contacto con el paciente).
- **Función:** Analizar la información recogida en la entrevista y generar una formulación clínica del caso.
- **Entrada:** Transcripción estructurada generada por el Agente 1.
- **Salida:** Informe de formulación de caso para Beatriz que incluye:
  - Datos del paciente (edad, contexto).
  - Diagnóstico diferencial: burnout, depresión, ansiedad (o combinaciones).
  - Identificación de estresores (carga laboral, falta de comunidad, conflicto de valores, etc.).
  - Estilo de apego identificado (seguro, ansioso, evitativo, desorganizado).
  - Creencias núcleo detectadas.
  - Fase actual del estrés (lucha, huida, congelación).
  - Justificación clínica de cada conclusión.

#### Agente 3: Planificador Terapéutico

- **Interacción:** Solo interna (sin contacto con el paciente).
- **Función:** Diseñar un plan de tratamiento basado en la formulación del caso.
- **Entrada:** Informe de formulación generado por el Agente 2.
- **Salida:** Plan terapéutico para Beatriz que incluye:
  - Objetivos terapéuticos priorizados.
  - Intervenciones recomendadas con secuenciación (qué primero, qué después, qué en paralelo).
  - Enfoque terapéutico sugerido.
  - Indicadores de progreso.
  - Consideraciones especiales basadas en el perfil del paciente.

### 2.2 Stack Tecnológico

| Componente | Tecnología |
|---|---|
| Modelo de IA | Claude (Anthropic API) |
| Interfaz paciente | Bot de Telegram (Telegram Bot API) |
| Backend | Python |
| Orquestación de agentes | Claude Agent SDK / LangGraph |
| Base de datos | SQLite (MVP) / PostgreSQL (producción) |
| Almacenamiento de sesiones | Base de datos + memoria conversacional |
| Entrega de informes a Beatriz | Telegram (chat privado) / Email / Dashboard web |

---

## 3. Flujo de Uso

### 3.1 Flujo del Paciente

1. El paciente inicia una conversación con el bot de Telegram.
2. El bot muestra un **disclaimer**: *"Esta herramienta es una pre-evaluación orientativa. No sustituye el diagnóstico ni el tratamiento de un profesional de salud mental. Toda la información será revisada por tu terapeuta."*
3. El paciente acepta y comienza la entrevista.
4. El **Agente Entrevistador** conduce la entrevista por bloques (ver sección 4).
5. Al finalizar, el bot agradece al paciente e indica que su terapeuta revisará la información.

### 3.2 Flujo Interno (Invisible para el paciente)

6. La transcripción estructurada se envía al **Agente Formulador de Caso**.
7. El Agente Formulador genera el informe de formulación.
8. El informe de formulación se envía al **Agente Planificador Terapéutico**.
9. El Agente Planificador genera el plan terapéutico.
10. Ambos informes se envían a Beatriz (vía Telegram privado, email o dashboard).

---

## 4. Bloques de la Entrevista

La entrevista del Agente 1 se organiza en los siguientes bloques temáticos. Las preguntas específicas de cada bloque serán proporcionadas por Beatriz.

### Bloque 1: Motivo de Consulta
- Por qué busca ayuda.
- Qué espera del proceso.
- Situación actual percibida.

### Bloque 2: Síntomas
- Síntomas emocionales (agotamiento, irritabilidad, tristeza, ansiedad).
- Síntomas físicos (insomnio, dolores, fatiga, problemas digestivos).
- Duración y evolución de los síntomas ("¿Desde cuándo?").
- Impacto en el funcionamiento diario.

### Bloque 3: Trabajo / Contexto Laboral
- Tipo de trabajo y condiciones.
- Estresores laborales: sobrecarga, falta de autonomía, conflicto de valores, falta de comunidad, falta de reconocimiento, inequidad.
- Relación con compañeros y superiores.
- Evolución de la satisfacción laboral.

### Bloque 4: Familia y Relaciones
- Situación familiar actual.
- Calidad de las relaciones personales.
- Red de apoyo social.
- Conflictos relacionales activos.

### Bloque 5: Historia Personal y Apego
- Relación con figuras parentales en la infancia.
- Patrones de apego (cómo se vincula, cómo gestiona la cercanía y la distancia).
- Experiencias significativas tempranas.
- Creencias sobre sí mismo y los demás formadas en la infancia.

### Bloque 6: Cierre
- Resumen de lo hablado.
- Espacio para añadir algo más.
- Agradecimiento y próximos pasos.

### Lógica de la Entrevista

- El agente no sigue un guion rígido: adapta el orden y profundidad de las preguntas según las respuestas del paciente.
- Utiliza **algoritmos de clasificación** para ir evaluando en tiempo real si las respuestas apuntan más a burnout, depresión, ansiedad o combinaciones.
- Realiza preguntas de seguimiento cuando detecta información clínicamente relevante.
- Cada bloque tiene preguntas obligatorias y opcionales (de profundización).

---

## 5. Modelo de Datos

### 5.1 Paciente

```
Paciente {
  id: UUID
  telegram_id: string
  nombre: string (opcional)
  fecha_registro: datetime
  sesiones: [Sesion]
}
```

### 5.2 Sesión de Entrevista

```
Sesion {
  id: UUID
  paciente_id: UUID
  fecha_inicio: datetime
  fecha_fin: datetime
  estado: "en_curso" | "completada" | "abandonada"
  bloque_actual: string
  transcripcion: [Mensaje]
  datos_estructurados: JSON  // Respuestas categorizadas por bloque
}
```

### 5.3 Informes

```
FormulacionCaso {
  id: UUID
  sesion_id: UUID
  fecha_generacion: datetime
  diagnostico_diferencial: JSON
  estresores: [string]
  estilo_apego: string
  creencias_nucleo: [string]
  fase_estres: string
  justificacion: string
  contenido_completo: text
}

PlanTerapeutico {
  id: UUID
  formulacion_id: UUID
  fecha_generacion: datetime
  objetivos: [string]
  intervenciones: JSON
  enfoque: string
  secuenciacion: string
  contenido_completo: text
}
```

---

## 6. Requisitos No Funcionales

### 6.1 Privacidad y Seguridad
- Todos los datos de pacientes deben almacenarse cifrados en reposo.
- Las conversaciones se transmiten cifradas (Telegram ya proporciona TLS).
- Acceso a informes restringido exclusivamente a Beatriz.
- Cumplimiento básico con RGPD (consentimiento informado, derecho a eliminación de datos).

### 6.2 Disclaimer
- Mostrado obligatoriamente al inicio de cada sesión.
- Texto: *"Esta herramienta es una pre-evaluación orientativa y no sustituye el diagnóstico ni el tratamiento de un profesional de salud mental. Toda la información recogida será revisada por tu terapeuta."*
- El paciente debe aceptar explícitamente antes de continuar.

### 6.3 Rendimiento
- Tiempo de respuesta del bot: < 10 segundos por mensaje.
- Capacidad de mantener múltiples sesiones simultáneas.
- Persistencia del estado de la entrevista ante desconexiones (el paciente puede retomar donde lo dejó).

---

## 7. Entrenamiento de los Agentes

Beatriz proporcionará los siguientes materiales para configurar los prompts y el comportamiento de cada agente:

| Material | Agente destino | Uso |
|---|---|---|
| Preguntas por bloques | Agente 1 (Entrevistador) | Guía de la entrevista, preguntas obligatorias y opcionales |
| Criterios diagnósticos (burnout vs depresión vs ansiedad) | Agente 2 (Formulador) | Algoritmos de clasificación diferencial |
| Ejemplos de formulaciones de caso | Agente 2 (Formulador) | Few-shot learning, formato de salida |
| Protocolos de tratamiento | Agente 3 (Planificador) | Base de conocimiento terapéutico |
| Ejemplos de planes terapéuticos | Agente 3 (Planificador) | Few-shot learning, formato de salida |

---

## 8. MVP (Producto Mínimo Viable)

Para la primera versión funcional:

1. **Bot de Telegram operativo** con el flujo de disclaimer + entrevista.
2. **Agente Entrevistador** funcional con los bloques básicos y preguntas de Beatriz.
3. **Agente Formulador** que genera un informe de formulación de caso en texto.
4. **Agente Planificador** que genera un plan terapéutico en texto.
5. **Envío de informes** a Beatriz por Telegram (chat privado con el bot).
6. **Persistencia básica** de sesiones en SQLite.

### Fuera del MVP (futuro)
- Dashboard web para Beatriz con historial de pacientes.
- Comparativa de evolución entre sesiones.
- Exportación de informes en PDF.
- Integración con calendario de citas.
- Métricas y estadísticas de uso.
- Migración a PostgreSQL.

---

## 9. Estructura del Proyecto

```
agentes-burnout/
├── README.md
├── spec.md
├── requirements.txt
├── .env.example
├── config/
│   └── settings.py
├── agents/
│   ├── __init__.py
│   ├── interviewer.py        # Agente 1: Entrevistador
│   ├── case_formulator.py    # Agente 2: Formulador de Caso
│   └── treatment_planner.py  # Agente 3: Planificador Terapéutico
├── prompts/
│   ├── interviewer_system.md
│   ├── formulator_system.md
│   └── planner_system.md
├── bot/
│   ├── __init__.py
│   ├── telegram_bot.py       # Lógica del bot de Telegram
│   └── handlers.py           # Handlers de comandos y mensajes
├── models/
│   ├── __init__.py
│   ├── patient.py
│   ├── session.py
│   └── reports.py
├── database/
│   ├── __init__.py
│   └── db.py
├── utils/
│   ├── __init__.py
│   └── helpers.py
└── main.py
```

---

## 10. Dependencias Principales

```
python-telegram-bot       # Bot de Telegram
anthropic                 # API de Claude
sqlalchemy                # ORM para base de datos
pydantic                  # Validación de datos
python-dotenv             # Variables de entorno
```
