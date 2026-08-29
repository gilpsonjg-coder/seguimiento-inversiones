# Roadmap: Plataforma de Seguimiento de Inversiones Personales
Fecha: 2026-08-21

## La idea en una frase
Un artifact web de un solo usuario (Jairo) para registrar manualmente aportes y valoraciones de sus inversiones en distintos vehículos, y ver cuánto está rindiendo cada uno.

## La acción core
Registrar un movimiento (aporte o valoración) de un vehículo de inversión, con fecha, monto y moneda — y que el sistema calcule y muestre el rendimiento acumulado a partir de esos registros. Todo lo demás (gráficos, alertas, comparaciones) existe para hacer más útil ese dato.

## Fase 1 — Lanzamiento
| # | Feature | Por qué va primero | Depende de |
|---|---------|--------------------|------------|
| 1 | Crear vehículo de inversión (catálogo abierto: nombre + tipo libre) | Sin un vehículo no hay nada que registrar. Es el requisito mínimo antes de cualquier aporte. | — |
| 2 | Registrar aporte (fecha, monto, moneda) | Es la mitad de la acción core: sin aportes no hay "invertido" contra qué comparar. | #1 |
| 3 | Registrar valoración del activo (fecha, valor, moneda) | La otra mitad de la acción core: sin valoraciones no hay "valor actual" que comparar contra lo invertido. | #1 |
| 4 | Vista simple de rendimiento (% ganancia/pérdida acumulada por vehículo) | Es el primer momento en que Jairo obtiene la respuesta que motivó todo el proyecto: "¿cómo va esta inversión?". Se valida aquí, con un solo vehículo (a2censo) y 2-3 aportes reales, antes de escalar. | #2, #3 |

Con esto Jairo puede hacer exactamente lo que describe como "siguiente paso más pequeño": cargar a2censo, meter 2-3 aportes históricos, y ver un número de rendimiento coherente. Ese es el criterio de éxito de la Fase 1 — no se agrega nada más hasta confirmar que el cálculo simple es confiable.

## Fase 2 — Mejora
1. **Cálculo XIRR** (rendimiento anualizado preciso) — una vez que la vista simple está validada con datos reales, se agrega la vista precisa que sí maneja bien aportes irregulares en el tiempo. Depende de tener ya varios aportes/valoraciones cargados para poder verificar que el número tiene sentido.
2. **Consolidado en moneda base (COP)** con tasa de cambio manual por registro — necesario en cuanto Jairo empiece a cargar vehículos en USD o GBP, no antes (con un solo vehículo en una sola moneda no hace falta consolidar).
3. **Gráfico de tendencia por vehículo** (evolución del valor en el tiempo) — se vuelve útil cuando ya hay suficientes puntos de valoración cargados para dibujar una curva; con 2-3 datos de la Fase 1 todavía no aporta mucho.
4. **Acumulado total del portafolio** (total aportado vs. valor actual, todos los vehículos) — tiene sentido una vez que existe más de un vehículo cargado; es una extensión natural de la vista simple ya construida.
5. **Exportar datos (JSON/CSV)** — mitiga el riesgo de durabilidad de `window.storage` identificado en el contexto. Se vuelve más urgente en cuanto el historial empieza a acumular meses de datos reales, no el primer día.
6. **Manejo explícito de vehículos sin precio de mercado (inmuebles)** — marcar visualmente que el "valor actual" es una estimación subjetiva y no un precio verificado, para no confundirlo con vehículos de mercado.

## Backlog
- **Alertas de vehículo sin actualizar** (ej. "3 meses sin registrar valor") — útil, pero requiere que ya exista suficiente historial y varios vehículos activos para que la alerta tenga sentido; antes de eso es ruido.
- **Comparación entre vehículos** (cuál rinde más, dónde concentrar próximos aportes) — necesita XIRR confiable (Fase 2, #1) y varios vehículos con historial ya cargado; sin eso la comparación no tiene base.
- **Análisis de diversificación/riesgo del portafolio** (concentración por tipo de vehículo o moneda) — depende de tener el consolidado total (Fase 2, #4) funcionando primero.
- **Recordatorios automáticos vía Make/Notion** — es una decisión de arquitectura explícitamente fuera del MVP (contradice la decisión de registro 100% manual); se evalúa solo si la falta de disciplina de registro resulta ser un problema real, no antes.
- **Tasas de cambio automáticas** — feature de vanidad en esta etapa: el registro manual de tasa ya resuelve el problema real (consolidar en COP); automatizarlo no acerca a Jairo a la acción core, solo ahorra un campo.
- **Precios de mercado automáticos** (acciones, cripto) — mismo caso: es una comodidad, no una necesidad, mientras el volumen de vehículos siga siendo manejable manualmente.
- **Multiusuario (ej. Charlotte)** — requiere backend real con autenticación, arquitectura completamente distinta a un artifact con `window.storage`. Explícitamente fuera de alcance hasta que el MVP de un solo usuario esté validado.

## Siguiente paso
Convertir la Fase 1 en spec con /crear-specs, usando este roadmap como contexto.
