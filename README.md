# Ecosistema de Automatización IA — Gestión de Leads Inmobiliarios

**Entrega Final · Curso de Automatización IA**
**Autora:** Verónica Bibiloni — Coldwell Banker Seniority

## Descripción

Sistema de automatización de extremo a extremo que gestiona consultas de leads inmobiliarios: recibe un email, clasifica el lead con IA, lo registra en una base de datos, redacta una respuesta personalizada con IA y —tras la aprobación humana— la envía al cliente.

| Categoría | Herramienta |
|---|---|
| Orquestador | Make (2 escenarios) |
| Base de datos | Airtable (3 tablas vinculadas) |
| Procesamiento IA | OpenAI GPT-4o-mini (clasifica) + Anthropic Claude Sonnet 4.6 (redacta) |
| Canal de salida | Gmail (aviso HITL + envío al cliente) |

## Cómo funciona

**Escenario 1 — Procesamiento (automático):** Gmail (correo no leído) → OpenAI clasifica y devuelve JSON → Parse JSON → Airtable crea el lead → Claude redacta la respuesta → Airtable guarda la respuesta → Gmail envía aviso de aprobación. Un manejador de errores registra cualquier fallo de API en la tabla Log de Errores.

**Escenario 2 — Envío tras aprobación (Human-in-the-Loop):** Airtable detecta un cambio → filtro: solo leads en "Aprobado por humano" → Gmail envía la respuesta al cliente → Airtable marca el lead como "Contactado".

El punto de control del HITL es el campo Estado: el sistema no contacta a nadie hasta que una persona cambia el estado del lead a "Aprobado por humano".

## Contenido del repositorio

Documentación (PDF):
1. 1_Arquitectura.pdf — Mapa de arquitectura del sistema
2. 2_Manual_de_Datos.pdf — Esquema de tablas + esquemas JSON
3. 3_Matriz_de_Costos.pdf — Justificación de modelos y ahorro
4. 4_Seguridad_y_Resiliencia.pdf — Datos, errores y HITL
5. 5_Dashboard.pdf — Panel de control y KPIs

Archivos técnicos:
- blueprint_escenario_1.json — Flujo de procesamiento (Make)
- blueprint_escenario_2.json — Flujo de envío tras aprobación (Make)
- /screenshots/ — Capturas de evidencia del sistema funcionando

## Enlaces

- Dashboard (vista de control, solo lectura): https://airtable.com/appcEPSYI8IdQ9e9t/shrEaNH1Z1JvMCNEr
- Video demo (3 min): [pegar aquí el enlace al video]

## Evidencia del "camino infeliz" (resiliencia)

Se probó el sistema invalidando temporalmente la API key de OpenAI. El flujo no se detuvo: el error `[401] Incorrect API key provided` quedó registrado automáticamente en la tabla Log de Errores con su módulo y fecha. Captura incluida en /screenshots/.
