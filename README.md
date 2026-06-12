# Sistema MP 2026 — Gestión de Mantención de Equipos Médicos

Programa de apoyo a la gestión de mantención preventiva (MP) y correctiva de equipamiento médico hospitalario. Funciona como **registro paralelo no oficial** que complementa la planilla Excel oficial validada por resolución, capturando lo que esta no registra: fecha real de ejecución, ejecutor, estado operativo del equipo y trazabilidad correctiva completa.

Es un archivo HTML autónomo: se abre en cualquier navegador, sin instalación. Los datos se guardan de forma persistente en el navegador y se respaldan mediante exportación a Excel.

## Principios de diseño

1. **La planilla oficial manda.** El Excel validado por resolución es la única fuente oficial; el programa nunca lo reemplaza. Si un dato está en el Excel, es porque su documentación fue verificada y archivada en la carpeta física del equipo.
2. **Nada se edita a mano.** El estado del equipo se deriva de los hechos registrados (resultados de MP, hitos correctivos), nunca por edición directa.
3. **Todo hito lleva fecha y responsable**, y se ordena cronológicamente aunque se registre en desorden.
4. **Identificación única** por N° de inventario o N° de serie (los números de serie se preservan tal cual, incluidos ceros a la izquierda). El ID es solo el orden en la planilla.

## Vistas

| Vista | Contenido |
|---|---|
| **Equipos** | Inventario completo: identificación, estado derivado, MP pendientes, última actualización y mini carta Gantt del año. Botón **Falla** para abrir caso correctivo. |
| **Plan MP** | Un evento de mantención por fila: mes, programa, resultado, registro no oficial (fecha, ejecutor, operativo) y botón **Registrar**. Selección múltiple para registro en lote. |
| **Correctivo** | Casos de falla: folio SIGEM, ingeniero, estado derivado de hitos, días sin novedad y alertas de seguimiento. |

Todas las vistas tienen **filtros tipo Excel** por columna (selección múltiple con buscador) y **barra de búsqueda global** sobre toda la tabla. El nombre de cada equipo abre su **ficha técnica** (espejo digital de la carpeta física): identificación, resumen del año y línea de tiempo cronológica con MP y correctivo intercalados.

## Códigos oficiales

**Programa (P):** `X` programada · `R` reprogramada · `RA` reprogramada de año anterior · `PM` puesta en marcha. `R*` indica evento no oficial creado por el programa.

**Resultado (R):** `Si` realizada · `Si-RA` de año anterior realizada · `C1–C8` causal de reprogramación · `FS` fuera de servicio · `No` no realizada · `NU` no ubicable · `Baja` equipo dado de baja.

**Causales:** C1 equipo ocupado por paciente · C2 en servicio técnico · C3 espera de repuestos · C4 en préstamo · C5 sin HH funcionario SEC · C6 sin HH servicio técnico externo · C7 ausencia funcionario SEC · C8 contingencia hospitalaria.

**Reglas de reprogramación:** Grupo A (C2, C3, C4) sin nueva fecha — se registra al reintegro del equipo. Grupo B (C1, C5–C8) reprogramación a 30 días — la X y la causal quedan en el mes original, R en el mes de destino (el programa lo automatiza).

## Flujo de trabajo preventivo

1. Importar la **planilla oficial del día** (botón ⬆ Planilla .xlsm, hoja `Registro_MP-2026`).
2. Por cada documento recibido: buscar el equipo (típicamente por N° de inventario) y **Registrar**: fecha, ejecutor, resultado, estado operativo (solo si el resultado es Si/Si-RA) y observaciones. El formulario recuerda fecha/ejecutor para registrar en cadena; varios equipos de un mismo informe pueden registrarse **en lote**.
3. Si la ejecución fue en un mes distinto al programado, el programa mantiene la X original, crea el evento **R** en el mes real y marca **documento pendiente 📄** (reprogramación por generar).
4. Si se registra una causal del Grupo B, el programa ofrece crear el evento R del mes siguiente (regla 30 días).
5. Verificada la documentación completa → archivar en la **carpeta física** → actualizar el **Excel oficial**. Al reimportar la planilla, la discrepancia se cierra sola.

## Indicadores y alertas

- **Δ Discrepancias**: registrado en el programa, la planilla aún no lo refleja → cola de oficialización (verificar, archivar, actualizar Excel).
- **⚠ Conflictos**: el resultado registrado difiere del de la planilla (ambos con valor). Resolución desde Editar: revisar la carpeta física; si la carpeta confirma la planilla, se corrige el registro; si confirma el registro, se corrige el Excel y la alerta queda silenciada hasta reimportar.
- **📄 Docs. pendientes**: registros con documento por generar (típicamente la reprogramación).
- **Perseguir ST ⚠**: casos correctivos con más de 20 días sin novedad.
- **Pendientes a Jun / Cumplimiento**: MP programadas sin ejecutar al mes actual y % global.

Todos los indicadores son clickeables y filtran la vista correspondiente.

## Flujo correctivo

1. **Falla**: desde la vista Equipos, abrir caso con folio SIGEM, fecha, ingeniero asignado y descripción. El equipo pasa a "No operativo".
2. **Resolución por hitos**: Compra de repuestos (compra ágil / trato directo con informe técnico) → Orden de compra (n°, empresa) → Envío a servicio técnico (n° envío, empresa, folio; estado "En servicio técnico") → Visitas técnicas (diagnóstico) → Retorno (guía de despacho; vuelve a "No operativo" hasta verificar).
3. **Supervisión**: a los 20 días sin novedad se activa la alerta "Perseguir ST"; la respuesta del ingeniero se registra como **Seguimiento** en la bitácora del caso.
4. **Entrega**: cierra el caso y deja el equipo "Operativo". Si el equipo tiene una MP con causal C2/C3/C4 sin ejecutar, el programa avisa ejecutarla de inmediato y crea el evento R en el mes actual.

## Datos: importación, exportación y respaldo

- **⬆ Planilla .xlsm**: carga la planilla oficial diaria. Los registros propios se conservan (anclados a inventario/serie + mes).
- **⬆ Backup registros**: restaura un Excel exportado previamente (registros, eventos no oficiales, casos correctivos y bitácora, sin duplicar).
- **⬇ Exportar registros**: genera el Excel de respaldo con hojas **Registros MP**, **Correctivo** y **Bitácora**.
- **● REC**: graba la actividad de uso (búsquedas, filtros, registros, importaciones) en una bitácora persistente, incluida en la exportación, para análisis y mejora del programa.

> **Hábito clave:** exportar el backup al terminar cada jornada. Al abrir una versión nueva del programa o cambiar de equipo/navegador: importar primero el backup y luego la planilla del día.

## Hoja de ruta

- **Fase 1 — Preventivo**: completada.
- **Fase 2 — Correctivo**: completada (pendiente validación con casos reales).
- **Fase 3a — Confiabilidad**: hoja "Para actualizar Excel" en la exportación + aviso de backup. Propuesta.
- **Fase 3b — Ficha técnica**: completada.
- **Fase 3c — Pendientes**: por definir.
- **Fase 4 — Informes**: cumplimiento mensual por servicio/ejecutor para jefatura. Propuesta.
