# Seguimiento de Inversiones Personales

Herramienta de un solo usuario para registrar manualmente aportes y valoraciones de
inversiones (a2censo, CDTs, acciones, inmuebles, etc.) y ver de un vistazo cuánto ha
ganado o perdido cada vehículo.

## Uso

Es una sola página autocontenida sin backend ni dependencias de build. Basta con abrir
[`index.html`](index.html) en el navegador, o servirlo con GitHub Pages.

Todos los datos se guardan únicamente en el `localStorage` del navegador que la abre —
no hay servidor ni sincronización entre dispositivos.

## Qué hace (Fase 1)

- Crear vehículos de inversión con nombre y tipo libre (catálogo abierto).
- Registrar aportes y valoraciones por vehículo, con fecha, monto y moneda (COP, USD, GBP).
- Editar y borrar cualquier registro (con confirmación antes de borrar).
- Ver por vehículo: total aportado, última valoración y % de rendimiento acumulado
  (última valoración vs. total aportado). Si aún no hay valoración, no se muestra
  ningún porcentaje.
- Dashboard consolidado: participación de cada vehículo sobre el capital aportado,
  agrupado por moneda (sin conversión entre monedas).

## Qué no hace todavía (Fase 2 / backlog)

Ver [`Salidas/roadmap.md`](Salidas/roadmap.md): cálculo XIRR, consolidado en una moneda
base, gráficos de tendencia en el tiempo, exportación de datos (JSON/CSV), alertas y
comparación entre vehículos.

## Contexto del proyecto

- [`contexto-proyecto-seguimiento-inversiones.md`](contexto-proyecto-seguimiento-inversiones.md) — propósito, alcance y riesgos identificados.
- [`spec.md`](spec.md) — spec funcional de la Fase 1.
- [`Salidas/roadmap.md`](Salidas/roadmap.md) — roadmap completo (Fase 1, Fase 2, backlog).
