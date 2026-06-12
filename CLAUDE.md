# CLAUDE.md — Sistema MP 2026 · Mantención de Equipos Médicos (HRT)

## Qué es este proyecto

Sistema MP 2026 es una aplicación de un solo archivo `Sistema_MP_2026.html`
que usa el encargado de la unidad de equipos médicos del hospital para
gestionar la mantención preventiva (MP) anual de 966 equipos biomédicos
(~2.350 eventos MP) y el ciclo correctivo completo. Es un **registro
paralelo no oficial**: la planilla Excel oficial validada por resolución
(`Programacion_MP_2026.xlsm`, hoja `Registro_MP-2026`) sigue siendo el
registro oficial final; el programa captura lo que ella no registra (fecha
real, ejecutor, estado operativo, trazabilidad) y gestiona las diferencias
sin ocultarlas. Detalle de uso en `README.md`; historia y reglas acumuladas
en `HISTORIAL.md`.

**Decisión 12-06-2026:** el usuario eligió continuar este programa y NO el
anterior ("GEB", index.html de 11 pestañas, descrito por versiones previas
de este archivo). Si aparecen referencias a GEB, son herencia: este archivo
manda.

## Contrato de trabajo con el usuario

- El usuario NO es programador. Él conoce su trabajo y sus necesidades;
  Claude aporta el criterio técnico (programación, diseño web, UX/UI,
  gestión de proyectos y planificación). La decisión final siempre es suya.
- Responder SIEMPRE en español. Probar con sus respaldos reales (carpeta
  `respaldos/`) antes de entregar. Tras cada cambio: commit + push +
  entregarle `Sistema_MP_2026.html`.
- **Él nutre, Claude razona.** El usuario entrega ideas y ejemplos de cómo lo
  haría a mano ("en Excel yo haría…"): eso es modelo mental para entender la
  lógica, NO un encargo literal. La solución se propone DENTRO del programa;
  no generar entregables paralelos cuando lo pedido debe vivir en él.
  Entender y preguntar antes de actuar.

## Flujo real del proceso (levantado el 11-06-2026 — reglas firmes)

- **El trabajo es una cadena de ciclos**: MP programada → ejecución o causal
  → papel → firma → carpeta física del equipo → recién al final el Excel
  oficial. El desorden = ciclos abiertos invisibles. Toda vista nueva debe
  mostrar el ciclo y su siguiente paso, no solo datos sueltos.
- **Orden de registro**: 1º el programa con el detalle completo (fecha,
  ejecutor, qué se hizo). 2º papel firmado y archivado en la carpeta del
  equipo. 3º RECIÉN AHÍ el resultado al Excel oficial. Si un dato está en el
  Excel es porque su documentación ya está archivada (por eso se descartó un
  estado intermedio "archivado": sería redundante). Celda vacía en el Excel
  = ciclo abierto; la cola de oficialización son las **Δ discrepancias**.
- **Reprogramación implícita**: resultado vacío + ejecución en mes posterior
  ⇒ reprogramación de facto. El programa ya la detecta (evento `R*` no
  oficial en el mes real + marca 📄 documento pendiente).
- **Revisión caso a caso, papeleo en lote**: la causal la decide SIEMPRE el
  usuario, caso a caso, con la evidencia al lado. Solo el papeleo se agrupa
  (imprimir en lote, repartir). La impresión es la última etapa, nunca de a
  uno.
- **Los 3 papeles** (procedimiento oficial PR-DC-0113/EQ2.1 v10): protocolo
  MP (Anexo 2 por familia; firma técnico ejecutor + jefe SEC + VºB
  servicio), reporte de reprogramación (Anexo 3; jefe SEC + supervisora; el
  formato oficial admite VARIOS equipos del mismo servicio), retiro de
  circulación (Anexo 4; jefe SEC + supervisora). El programa aún NO imprime
  anexos. **Pregunta abierta antes de construir impresión**: ¿el archivo en
  carpetas individuales exige hoja por equipo o sirven copias del Anexo 3
  grupal?
- **Portador del papel**: cada documento impreso lleva un portador asignable
  (por defecto quien ejecutó esa MP). Al construir papeleo, recordar qué
  firmas verificar y a quién.

## Método de trabajo (cómo avanzamos sin retroceder)

- **Fases.** Todo trabajo nuevo se divide en fases pequeñas registradas en
  `PLAN.md` (por decidir / pendiente / en curso / hecha). Una fase a la vez:
  la siguiente no se abre sin que el usuario valide la anterior.
- **Anti-regresión.** Lo que ya funciona es intocable salvo acuerdo explícito.
  Antes de dar una fase por terminada: probar con los respaldos reales y
  verificar que las vistas y flujos existentes siguen funcionando. Si algo
  se rompe, revertirlo es la prioridad número uno. Commits pequeños con
  mensaje claro.
- **Entender antes de ejecutar.** Si el "para qué" de una petición no es
  evidente y no está en este archivo, hacer 2 a 4 preguntas concretas, nunca
  más. Si existe una forma mejor de lograr el fin real, proponerla en 2-3
  líneas junto a lo pedido; el usuario decide. Si una solicitud duplica algo
  que ya existe o haría retroceder el programa, NO ejecutarla a ciegas:
  decirle dónde ya puede ver/hacer eso y proponer la alternativa.
- **Comunicación.** Respuestas cortas y al grano; nada de muros de texto ni
  listas de todo lo posible. Cero explicaciones técnicas salvo que el usuario
  las pida o las necesite para decidir (máximo 3 líneas, lenguaje simple). Al
  terminar algo, indicar cómo probarlo en 1-2 pasos.
- **Aprendizaje.** Cada decisión nueva, preferencia o "para qué" descubierto
  se anota en este mismo archivo al cerrar la fase, y el avance se refleja en
  `PLAN.md`. Estos dos archivos son la memoria del proyecto entre sesiones:
  mantenerlos al día es parte del trabajo, no un extra.

## Arquitectura (decisiones firmes)

- Un solo archivo `Sistema_MP_2026.html` (~360 KB), JavaScript vanilla,
  3 vistas (Equipos, Plan MP, Correctivo), ~65 funciones. Los datos de la
  planilla van embebidos en el archivo (`const DATA`) y se actualizan
  importando el `.xlsm` del día.
- **Única dependencia externa**: SheetJS 0.18.5 desde CDN para leer/escribir
  Excel (requiere internet; embeberla está en PLAN.md). Google Fonts solo
  estética. Nada más sin acuerdo explícito con el usuario.
- **Persistencia**: dentro de claude.ai usa `window.storage` (API del
  entorno); como archivo local usa `localStorage` mediante un shim con la
  misma interfaz (primeras líneas del script). Claves: `datos-mp-2026`,
  `registros-mp-2026`, `virt-mp-2026`, `casos-mp-2026`, `bitacora-mp-2026`.
  Ambos almacenes NO se comunican entre sí: el puente entre entornos es la
  exportación/importación de respaldos Excel (hábito clave del README).
- Identificación de registros anclada a N° inventario/serie + mes para
  sobrevivir a las importaciones diarias de la planilla. Estado del equipo
  siempre derivado, nunca manual.
- Repositorio GitHub `cribeltr/Hospital-Regional`; respaldos reales del
  usuario en `respaldos/` (planilla oficial + exportaciones), usarlos para
  probar.
