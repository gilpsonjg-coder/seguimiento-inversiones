# Spec: Plataforma de Seguimiento de Inversiones Personales — Fase 1
Fecha: 2026-08-21

## Overview
Un artifact web de un solo usuario (Jairo) para registrar manualmente los aportes y valoraciones de sus inversiones, organizadas por vehículo (a2censo, CDTs, acciones, inmuebles, etc.), y ver de un vistazo cuánto ha ganado o perdido en cada uno. No hay integración automática de precios ni tasas de cambio: todo el registro es manual. Esta Fase 1 es la base sobre la que después se construyen XIRR, consolidado multi-moneda y análisis de portafolio.

## Usuarios objetivo
Jairo Gil, único usuario. Hoy no tiene ningún sistema centralizado: sus inversiones (a2censo, CDTs, acciones, inmuebles, etc.) viven dispersas en la cabeza, en hojas sueltas o en las plataformas de cada vehículo, sin una vista consolidada de aportes vs. valor actual ni de rendimiento real.

## Alcance

### La v1 SÍ hace
- **Crear vehículo de inversión:** Jairo da de alta un vehículo con un nombre (ej. "a2censo - Operación XYZ") y un tipo libre de texto (no hay catálogo cerrado; el tipo lo escribe Jairo, ej. "a2censo", "CDT", "Acciones", "Inmueble").
- **Registrar aporte:** para un vehículo existente, Jairo registra una fecha, un monto y una moneda (COP, USD o GBP) cada vez que invierte dinero.
- **Registrar valoración:** para un vehículo existente, Jairo registra en cualquier momento el valor actual del activo, con su fecha y moneda — independiente de los aportes.
- **Editar y borrar registros:** cualquier aporte o valoración ya registrado se puede corregir (cambiar fecha, monto o moneda) o eliminar por completo, para el caso de errores de digitación.
- **Vista de rendimiento por vehículo:** para cada vehículo, Jairo ve el total aportado, la última valoración registrada (si existe) y el % de ganancia/pérdida acumulada calculado como (última valoración − total aportado) / total aportado. Si aún no hay ninguna valoración registrada, la vista muestra en su lugar el acumulado aportado y el listado de aportes, sin inventar un % de rendimiento.

### La v1 NO hace
- No calcula XIRR ni ningún rendimiento anualizado — eso es Fase 2.
- No consolida el portafolio en una moneda base (COP) ni convierte monedas — cada vehículo se ve en su propia moneda. Eso es Fase 2.
- No dibuja gráficos de tendencia en el tiempo — Fase 2.
- No calcula el acumulado total de todo el portafolio (solo vehículo por vehículo) — Fase 2.
- No trae precios de mercado ni tasas de cambio automáticas — decisión explícita del proyecto, no solo de esta fase.
- No tiene alertas, recordatorios, ni comparación entre vehículos — backlog.
- No soporta más de un usuario ni login — fuera de alcance del proyecto completo, no solo de esta fase.
- No exporta datos (JSON/CSV) todavía — Fase 2.

## Comportamiento esperado
1. Jairo abre la plataforma y ve la lista de vehículos que ya ha creado (vacía la primera vez).
2. Jairo crea un vehículo nuevo dando un nombre y un tipo (texto libre). El vehículo aparece en la lista.
3. Jairo entra a un vehículo y registra un aporte: fecha, monto, moneda. El aporte queda listado dentro del vehículo, junto con los aportes anteriores.
4. En cualquier momento (no solo al aportar), Jairo entra al vehículo y registra una valoración: fecha, valor, moneda. La valoración queda listada junto con las anteriores.
5. Dentro del vehículo, Jairo ve un resumen: total aportado, última valoración (si existe) y el % de rendimiento acumulado. Si todavía no hay valoración, ve el total aportado y el listado de aportes, sin ningún porcentaje.
6. Jairo puede volver a cualquier aporte o valoración ya cargado, editarlo si se equivocó, o borrarlo. El resumen del vehículo se recalcula automáticamente al editar o borrar.
7. Jairo repite este proceso para varios vehículos distintos (a2censo, CDT, etc.), cada uno con su propio historial y resumen independiente.

## Errores y seguridad
- **Vehículo sin ningún dato todavía:** se muestra vacío, invitando a registrar el primer aporte o valoración, sin errores ni cálculos.
- **Vehículo con aportes pero sin valoración:** no se muestra ningún % de rendimiento (evita mostrar un número engañoso); se muestra el acumulado aportado y el listado de aportes.
- **Montos o fechas inválidas:** el formulario de aporte/valoración no permite guardar montos negativos, montos vacíos o fechas vacías; debe pedir corregir el dato antes de guardar.
- **Borrar un registro:** antes de eliminar un aporte o valoración, se pide confirmación explícita (no se borra con un solo clic accidental), ya que no hay forma de deshacer.
- **Datos financieros personales:** toda la información vive únicamente en el navegador de Jairo (almacenamiento local del artifact); no se envía a ningún servidor externo ni se comparte con nadie.
- **Huecos en el tiempo:** el sistema no asume nada sobre periodos sin registros — simplemente muestra la última valoración conocida, sin proyectar ni inventar valores intermedios.

## Éxito
La Fase 1 se considera exitosa cuando Jairo ha cargado **varios vehículos distintos** (no solo uno) — por ejemplo a2censo, un CDT, y al menos un tercero — cada uno con sus aportes y valoraciones reales, y en cada uno el resumen (total aportado, última valoración, % de rendimiento) refleja correctamente lo que Jairo esperaría calculando a mano. Esto confirma que el modelo de datos y el cálculo simple son confiables antes de construir XIRR y el resto de la Fase 2 encima.

## V2 (opcional)
(Corresponde a la Fase 2 y Backlog del roadmap — no se construye en esta spec)
- Cálculo XIRR (rendimiento anualizado preciso).
- Consolidado en moneda base (COP) con tasa de cambio manual por registro.
- Gráfico de tendencia por vehículo.
- Acumulado total del portafolio (todos los vehículos).
- Exportar datos (JSON/CSV).
- Manejo visual explícito de vehículos sin precio de mercado objetivo (inmuebles).
- Alertas de vehículo sin actualizar.
- Comparación de rendimiento entre vehículos.
- Análisis de diversificación/riesgo del portafolio.
- Recordatorios automáticos, tasas de cambio automáticas, precios de mercado automáticos, multiusuario.
