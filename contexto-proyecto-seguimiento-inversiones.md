# Plataforma de Seguimiento de Inversiones Personales — Contexto del Proyecto

## 1. Propósito

Jairo Gil necesita una herramienta para llevar el control de sus inversiones personales a través de distintos vehículos financieros. Hoy no existe un sistema centralizado: se necesita registrar aportes, valorar activos en el tiempo, y entender el rendimiento real de cada inversión para tomar mejores decisiones sobre dónde seguir invirtiendo.

## 2. Alcance del MVP

Un **artifact web de un solo usuario** (Jairo), con persistencia mediante `window.storage` (datos personales, no compartidos), sin backend ni autenticación. No contempla multiusuario en esta fase — ver sección 8 (Visión futura).

## 3. Vehículos de inversión

Debe soportar **cualquier tipo de vehículo**, incluyendo (lista abierta, no cerrada):
- a2censo (deuda/factoring)
- Acciones, ETFs, cripto
- CDTs
- Fondos de inversión
- Inmuebles
- Otros que Jairo agregue manualmente

Cada vehículo debe poder crearse como una entrada nueva por el usuario, sin necesidad de que el tipo esté predefinido en el código (catálogo abierto).

## 4. Registro de datos

Todo el registro es **manual** (decisión explícita — no hay integración automática de precios de mercado en el MVP):

- **Aportes:** cada vez que Jairo invierte, registra fecha, vehículo, monto y moneda.
- **Valor del activo:** en cualquier momento (no solo al aportar), Jairo puede registrar el valor actual del activo para ese vehículo, con fecha y moneda.
- **Sin recordatorios:** no hay notificaciones ni recordatorios automáticos. Jairo registra cuando se acuerda o cuando hace un movimiento. El sistema debe tolerar huecos temporales en el historial sin romperse (ver riesgo en sección 7).

## 5. Monedas

Multi-moneda: **COP, USD, GBP** (y posiblemente otras a futuro). Para el MVP:
- Cada aporte y cada valoración se registra en su moneda original.
- Existe una **moneda base** (COP) para consolidar el acumulado total del portafolio.
- La tasa de cambio para convertir a la moneda base se **ingresa manualmente** junto con cada registro (no hay integración automática de tasas en el MVP). Mejora futura: traer tasa de cambio automática.

## 6. Cálculo de rendimiento

Debe combinar dos niveles:

1. **Vista simple** (a primera vista): % de ganancia/pérdida acumulada por vehículo, fácil de leer.
2. **Vista precisa** (al profundizar): cálculo financiero correcto tipo **XIRR** (tasa interna de retorno anualizada), que sí maneja bien aportes irregulares en el tiempo — a diferencia de un simple "(valor final - invertido) / invertido", que distorsiona el resultado cuando hay múltiples aportes en fechas distintas.

Consideración importante por tipo de vehículo:
- Vehículos con valorización de mercado frecuente (acciones, fondos, cripto) → curva de valor más rica.
- Vehículos con tasa fija y vencimiento (CDTs) → el rendimiento es prácticamente determinístico, pero debe registrarse igual para mantener consistencia y comparabilidad.
- Vehículos sin precio de mercado objetivo (inmuebles) → el "valor actual" es una estimación manual de Jairo (avalúo o percepción), y el sistema debe dejar claro que ese dato es subjetivo, no un precio de mercado verificado.

## 7. Visualización y análisis

- **Gráficos de tendencia:** evolución del valor de cada vehículo en el tiempo (crecimiento/decrecimiento).
- **Acumulado por vehículo:** total aportado vs. valor actual, desde el primer aporte hasta el último corte.
- **Recomendaciones basadas en reglas** (no necesariamente IA compleja en el MVP), construidas de manera incremental:
  1. Alertas simples (ej. "este activo lleva 3 meses sin actualizarse" o "sin crecimiento").
  2. Comparación entre vehículos (cuál está rindiendo más, dónde tiene sentido concentrar próximos aportes).
  3. Análisis de diversificación/riesgo del portafolio total (concentración por tipo de vehículo o moneda).

## 8. Visión futura (fuera del MVP)

- **Multiusuario:** posibilidad de que otras personas (ej. Charlotte) usen la plataforma con sus propios datos privados. Esto requiere backend real con autenticación — no es viable solo con `window.storage` de un artifact. Se abordará como una fase posterior, con arquitectura distinta.
- **Recordatorios automáticos:** posible integración con Make/Notion (como el Tablero Gerencial de BASCOSTA) para notificar registros pendientes.
- **Tasas de cambio automáticas:** traer tasa de cambio en vivo en lugar de ingreso manual.
- **Precios de mercado automáticos:** para vehículos con cotización pública (acciones, cripto), traer el valor automáticamente en lugar de digitarlo.

## 9. Riesgos identificados

1. **Precisión del cálculo XIRR:** con aportes irregulares en múltiples monedas, un error en fechas o tasas de cambio puede generar un rendimiento mostrado que sea engañoso — justo el dato que Jairo usaría para decidir dónde invertir más.
2. **Durabilidad de los datos:** `window.storage` en un artifact no garantiza persistencia a largo plazo ni backup fácil. Si esto va a acumular años de historial financiero, se necesita un plan de exportación/respaldo (ej. exportar a JSON o CSV periódicamente) desde el inicio.
3. **Huecos en el registro:** al no haber recordatorios, el historial dependerá 100% de la disciplina manual de Jairo. Los cálculos de rendimiento y las gráficas deben diseñarse para tolerar periodos sin datos sin romper la lógica ni mostrar información engañosa.

## 10. Siguiente paso más pequeño para validar

Construir un artifact mínimo con **un solo vehículo de inversión** (a2censo, ya conocido a fondo por Jairo), permitir registrar 2-3 aportes históricos reales con sus fechas y valores, y verificar que el cálculo de rendimiento (simple + XIRR) arroje un número coherente antes de escalar a los demás vehículos y construir la interfaz completa.
