# Historial de desarrollo — Sistema MP 2026

Registro cronológico de las iteraciones del programa, con el ajuste solicitado y el porqué detrás de cada uno. Fecha de desarrollo: 11 de junio de 2026.

## v1 — Base del sistema
**Solicitud:** sistema a partir de la planilla oficial con vistas Equipos y Plan MP, filtros tipo Excel por columna y búsqueda global en ambas vistas.
**Implementado:** lectura de la hoja `Registro_MP-2026` (966 equipos, ~2.350 eventos MP), vista Equipos con estado derivado, pendientes, última actualización y mini carta Gantt anual; vista Plan MP con un evento por fila; indicadores de cumplimiento.
**Decisión técnica:** se usó `Registro_MP-2026` como fuente (y no `PMP_2026`) porque contiene las subcolumnas P/R completas por mes.

## v2 — Ejecutor en blanco
**Solicitud:** no extraer el ejecutor de la planilla; se definirá después.
**Porqué:** el "Responsable MP" de la planilla no corresponde al dato que se quiere gestionar.

## v3 — Definición del método de trabajo
**Acuerdos:** trabajo iterativo (ajuste → implementación → pregunta del "para qué"); la planilla es el documento oficial validado por resolución y se sube a diario; el programa es un **registro no oficial** de los documentos recibidos, que puede diferir de la planilla (las diferencias se gestionan, no se ocultan).

## v4 — Registro de mantenciones preventivas
**Solicitud:** botón por evento que pida fecha, observaciones opcionales, ejecutor (lista de los 11 técnicos del hospital + Personal Externo) y pregunta equipo operativo Sí/No.
**Porqué:** la planilla oficial solo registra resultados; no tiene fecha, ejecutor ni estado del equipo. Registro persistente anclado a inventario/serie + mes para sobrevivir a las importaciones diarias.

## v5 — Importar, exportar y grabación
**Solicitud:** botón para importar el .xlsm, botón para exportar los registros a Excel y botón de grabación de actividad.
**Implementado:** importación de la planilla diaria en el navegador, exportación de respaldo (Registros MP + Bitácora) y botón REC que registra búsquedas, filtros, registros e importaciones para que el desarrollo aprenda del uso real.

## v6 — Aprendizaje de la bitácora: lote, memoria y discrepancias
**Hallazgo (bitácora):** el flujo real es buscar por N° de inventario documento por documento; un mismo informe cubre varios equipos con igual fecha y ejecutor; hubo corrección de fecha por la precarga "hoy".
**Implementado:** memoria del formulario (última fecha/ejecutor), registro en lote con selección múltiple, e indicador **Δ Discrepancias** (registrado en el programa, planilla aún sin reflejo). Se incorporó además el **tipo de evento**: MP programada (X) o reprogramada (R/RA).

## v7 — Restauración de backup
**Solicitud:** poder importar el respaldo para no perder lo registrado.
**Ajuste posterior:** son dos archivos diferentes → dos botones separados (Planilla .xlsm / Backup registros), cada uno valida su hoja correspondiente.

## v8 — Reprogramaciones (caso 2-126872)
**Solicitud:** MP programada en abril, ejecutada el 29-05. El programa debe permitir registrarla, elegir el programa (X, R u otro) y generar el pendiente del documento faltante.
**Refinamiento del usuario:** debe **crearse un nuevo evento R en mayo**, manteniendo la X en abril, con el documento pendiente.
**Implementado:** detección de desfase mes programado vs mes de ejecución; creación automática del evento **R\*** (no oficial) en el mes real; la X original intacta; marca **📄 documento pendiente**; selector de Programa en el formulario; indicador "Docs. pendientes" clickeable.

## v9 — Resultado en el registro
**Solicitud:** lista desplegable con el resultado (Si, C1, etc.).
**Implementado:** selector con todos los códigos oficiales y su descripción; visualización del resultado registrado junto al oficial (con asterisco cuando es no oficial); las discrepancias pasan a comparar resultado registrado vs planilla.

## v10 — Operativo condicional y corrección de reglas
**Solicitud:** si el resultado es distinto de Si, no corresponde preguntar por el estado operativo.
**Hallazgos del análisis de datos:** eventos creados como X que debían ser R; patrón correcto en dos pasos (causal en mes original + Si en el evento R).
**Implementado:** pregunta de operatividad solo para Si/Si-RA; el evento de reprogramación se crea siempre como R; **regla 30 días Grupo B**: al registrar causal C1/C5–C8 se ofrece crear el evento R del mes siguiente (Grupo A C2/C3/C4 sin fecha nueva, según norma).

## v11 — Ciclo correctivo
**Solicitud:** especificación completa CICLO_CORRECTIVO (pseudocódigo).
**Implementado:** tercera vista **Correctivo**. Falla → caso con folio SIGEM, ingeniero y descripción (equipo "No operativo"). Resolución por hitos: Compra (ágil/trato directo), OC, Envío a ST (→ "En servicio técnico"), Visitas, Retorno (→ "No operativo" hasta verificar). Supervisión: alerta **"Perseguir ST"** a los 20 días sin novedad + bitácora de seguimientos. Entrega: cierre, equipo "Operativo" y verificación de MP con causal C2/C3/C4 sin ejecutar (aviso + evento R en el mes actual).
**Invariantes:** todo hito con fecha y responsable; orden cronológico aunque se registre en desorden; estado del equipo siempre derivado, nunca manual.

## v12 — Comprensión del flujo documental
**Aporte del usuario:** cada equipo tiene **carpeta física**; solo con la documentación completa se archiva y se actualiza el Excel; si algo falta, el Excel no se actualiza.
**Consecuencia de diseño:** las discrepancias Δ son la cola natural de trabajo (no errores). Se evaluó y **descartó** un estado intermedio "archivado en carpeta": si está en el Excel, está archivado — sería redundante.

## v13 — Conflictos y resolución vía carpeta
**Solicitud:** si el programa dice C3 y la planilla importada dice C2, debe generarse una alerta para revisar la carpeta y corregir donde corresponda.
**Implementado:** separación entre **Δ Discrepancia** (planilla sin resultado aún) y **⚠ Conflicto** (resultados distintos). Al editar un registro en conflicto: recuadro con ambos valores, referencia al N° de carpeta y dos resoluciones — "la carpeta confirma la planilla → corregir mi registro" o "la carpeta confirma mi registro → corregiré el Excel" (alerta silenciada hasta reimportar, donde se cierra sola al coincidir).

## v14 — Fases del proyecto
Corte de fases acordado: Fase 1 Preventivo ✔ · Fase 2 Correctivo ✔ · Fase 3 Pendientes (vista HOY descartada por ahora a pedido del usuario). Evaluación y propuesta presentadas: 3a Confiabilidad (hoja "Para actualizar Excel" + aviso de backup), 3b Ficha técnica, 3c Pendientes, 4 Informes.

## v15 — Ficha técnica por equipo (Fase 3b)
**Solicitud:** implementar 3b.
**Implementado:** el nombre del equipo es clickeable en las tres vistas y abre la **ficha técnica**: identificación completa (incluido N° de carpeta física), resumen del año (programadas/realizadas/causales/pendientes/casos abiertos) y línea de tiempo cronológica 2026 que intercala MP (oficial y registrado, con marcas 📄/⚠) e hitos y seguimientos correctivos.

## v16 — Documentación
**Solicitud:** generar HISTORIAL.md y README.md (este documento y la documentación de uso).

## v17 — Persistencia local (12-06-2026)
**Hallazgo:** el guardado usaba la API de almacenamiento de claude.ai (`window.storage`), que no existe al abrir el archivo en un navegador normal: fuera de claude.ai nada persistía entre sesiones y todo dependía de exportar el backup.
**Implementado:** respaldo automático con `localStorage` cuando `window.storage` no existe (misma interfaz, mismas claves `*-mp-2026`). Importación/exportación Excel intactas. Los almacenes de claude.ai y del navegador local no se comunican: el puente sigue siendo el backup Excel.

## v18 — Informe técnico en trato directo (12-06-2026)
**Solicitud:** si la compra de repuestos es por trato directo, el programa debe solicitar el N° de Informe Técnico.
**Implementado:** en el hito Compra, al elegir "Trato directo" aparece el campo **N° Informe Técnico** y es obligatorio para guardar; con "Compra ágil" el campo no se muestra ni se arrastra. El número queda en el detalle del hito (caso, ficha técnica y hoja Correctivo del respaldo).

## v19 — Grabación ampliada (12-06-2026)
**Solicitud:** que el botón de grabación grabe más cosas.
**Implementado:** la bitácora ahora graba además (1) la consulta de casos correctivos (folio, equipo, estado y días sin novedad, solo al abrirlos desde la tabla), (2) el inicio de cada sesión con su entorno (archivo local o claude.ai), y (3) la vista Correctivo correctamente identificada en búsquedas y cambios de vista (antes se anotaba como "Plan MP"). Lo ya grabado se mantiene igual.

---

### Reglas acumuladas vigentes
1. Fuente de datos: hoja `Registro_MP-2026`, desde fila 8; solo códigos válidos; series preservadas con ceros a la izquierda.
2. Columna Ejecutor: no se extrae de la planilla (se registra en el programa).
3. Registro MP: fecha + ejecutor + resultado + operatividad (solo Si/Si-RA) + observaciones; memoria de sesión; lote disponible.
4. Reprogramación por desfase: X se mantiene, evento R\* en el mes de ejecución, documento pendiente automático.
5. Causal Grupo B: ofrecer evento R al mes siguiente. Grupo A: sin fecha nueva.
6. Discrepancia Δ ≠ Conflicto ⚠; conflicto se resuelve contra la carpeta física.
7. Estado del equipo: derivado de resultados e hitos; nunca manual.
8. Backup: dos botones de importación separados (planilla / backup); exportación con Registros MP + Correctivo + Bitácora.
9. Persistencia: `window.storage` en claude.ai, `localStorage` como archivo local (claves `*-mp-2026`); el backup Excel es el puente entre entornos.
