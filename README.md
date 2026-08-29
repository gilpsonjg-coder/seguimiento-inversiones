# Seguimiento de Inversiones Personales

Herramienta de un solo usuario para registrar manualmente aportes y valoraciones de
inversiones (a2censo, CDTs, acciones, inmuebles, etc.) y ver de un vistazo cuánto ha
ganado o perdido cada vehículo.

## Uso

Es una sola página autocontenida sin backend ni dependencias de build. Basta con abrir
[`index.html`](index.html) en el navegador, o servirlo con GitHub Pages.

Todos los datos se guardan únicamente en el `localStorage` del navegador que la abre —
no hay servidor ni sincronización entre dispositivos.

## Qué hace

- Crear vehículos de inversión con nombre y tipo libre (catálogo abierto).
- Registrar aportes y valoraciones por vehículo, con fecha, monto, moneda (COP, USD,
  GBP) y una tasa de cambio a COP opcional por registro.
- Editar y borrar cualquier registro (con confirmación antes de borrar).
- Ver por vehículo: total aportado, última valoración, % de rendimiento simple
  (última valoración vs. total aportado) y TIR anualizada (XIRR) — que sí maneja bien
  aportes irregulares en el tiempo. Si aún no hay valoración, no se muestra ningún
  porcentaje.
- Gráfico de evolución por vehículo: aportado acumulado vs. valor registrado en el
  tiempo.
- Alertas de vehículos sin actualizar hace más de 3 meses.
- Comparación de rendimiento entre vehículos (por TIR cuando existe), con una
  recomendación simple de dónde concentrar próximos aportes.
- Diversificación del portafolio por tipo de vehículo y por moneda, con aviso cuando
  hay concentración alta (≥60 %) en un solo tipo.
- Dashboard con dos vistas: el portafolio total consolidado en COP (usando la tasa de
  cambio registrada en cada aporte/valoración) y el detalle por moneda original.
- Exportar todos los datos a JSON o CSV, e importar un JSON exportado para restaurar
  el estado completo (con confirmación antes de reemplazar los datos actuales) — es
  el mecanismo de respaldo, ya que todo vive en el `localStorage` del navegador.

## Backlog completo

El roadmap original ([`Salidas/roadmap.md`](Salidas/roadmap.md)) ya está implementado
por completo. Lo que sigue fuera de alcance por decisión de arquitectura (no por
prioridad): recordatorios automáticos, tasas de cambio o precios de mercado
automáticos, y multiusuario — todos requieren un backend real, que contradice el
diseño de esta herramienta como página autocontenida de un solo usuario.

## Contexto del proyecto

- [`contexto-proyecto-seguimiento-inversiones.md`](contexto-proyecto-seguimiento-inversiones.md) — propósito, alcance y riesgos identificados.
- [`spec.md`](spec.md) — spec funcional de la Fase 1.
- [`Salidas/roadmap.md`](Salidas/roadmap.md) — roadmap completo (Fase 1, Fase 2, backlog).
