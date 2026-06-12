# CLAUDE.md — GEB · Gestión de Equipos Biomédicos Críticos (HRT 2026)

## Qué es este proyecto

GEB es una aplicación de un solo archivo `index.html` que usa el encargado
de la unidad de equipos médicos del hospital para gestionar el mantenimiento
preventivo (MP) anual de 966 equipos biomédicos críticos, además de
correctivos, documentación firmada (anexos oficiales) y tareas pendientes.
El Excel institucional sigue siendo el registro oficial final; GEB aporta la
trazabilidad y el control diario que el Excel no entrega.

## Contrato de trabajo con el usuario

- El usuario NO es programador. Él conoce su trabajo y sus necesidades;
  Claude aporta el criterio técnico (programación, diseño web, UX/UI,
  gestión de proyectos y planificación). La decisión final siempre es suya.
- Responder SIEMPRE en español. Probar con sus respaldos reales antes de
  entregar. Tras cada cambio: commit + push (inicializar git si no existe)
  + entregarle `index.html`.
- **Él nutre, Claude razona.** El usuario entrega ideas y ejemplos de cómo lo
  haría a mano ("en Excel yo haría…"): eso es modelo mental para entender la
  lógica, NO un encargo literal. La solución se propone DENTRO de GEB; no
  generar entregables paralelos (planillas externas) cuando lo pedido debe
  vivir en el programa. Entender y preguntar antes de actuar.

## Flujo real del proceso (levantado el 11-06-2026 — reglas firmes)

- **El trabajo es una cadena de ciclos**: MP programada → ejecución o causal
  → papel → firma → carpeta física → recién al final el Excel oficial. El
  desorden del usuario = ciclos abiertos invisibles. Toda vista nueva debe
  mostrar el ciclo y su siguiente paso, no solo datos por pestaña.
- **Orden de registro**: 1º GEB con el detalle completo (fecha, ejecutor, qué
  se hizo — la trazabilidad que antes no existía). 2º papel firmado y
  archivado en la carpeta del equipo. 3º RECIÉN AHÍ el código de resultado al
  Excel oficial. Celda vacía en el Excel = ciclo abierto. La lista "falta
  traspasar al Excel" debería ofrecer solo lo que ya tiene papel archivado.
- **Rojo = papel pendiente**: causal registrada cuyo documento no está
  "archivado" en `S.docflow`. El semáforo debe verse donde se revisa.
- **Reprogramación implícita (detectable)**: resultado vacío + ejecución en un
  mes posterior ⇒ hubo reprogramación de facto → falta registrar la causal e
  imprimir su Anexo 3. GEB debe marcarlo solo.
- **Revisión caso a caso, papeleo en lote**: la causal la decide SIEMPRE el
  usuario, caso a caso, con la evidencia al lado (correctivo abierto, estado,
  préstamo, familia externa, fichas, ejecución posterior). Solo el papeleo se
  agrupa: llenar todos los vacíos → filtrar rojos → por servicio → imprimir
  en lote → repartir. La impresión es la última etapa, nunca de a uno.
- **Portador del papel**: cada documento impreso lleva un portador asignable
  (por defecto quien tenía asignada/ejecutó esa MP; a veces el usuario).
  "Hoy" debe recordar qué firmas verificar y a quién.
- **Los 3 papeles** (procedimiento oficial PR-DC-0113/EQ2.1 v10, PDF en la
  carpeta del proyecto): protocolo MP (Anexo 2 por familia; firma técnico
  ejecutor + jefe SEC + VºB servicio), reporte de reprogramación (Anexo 3;
  jefe SEC + supervisora; el formato oficial admite VARIOS equipos del mismo
  servicio, cada fila con su causal), retiro de circulación (Anexo 4; jefe
  SEC + supervisora). `formReprog` hoy imprime una hoja por equipo.
  **Pregunta abierta antes de tocar impresión**: ¿el archivo en carpetas
  individuales exige hoja por equipo o sirven copias del Anexo 3 grupal?
- **Próxima fase propuesta (espera OK)**: vista **"Revisión y cierre"** en
  Plan MP — todos los vacíos de meses pasados en una lista única, evidencia
  automática, causal asignable en línea, semáforo rojo de papel; conectada a
  Documentos (lote y estados) y al traspaso a Excel condicionado a papel
  archivado. Después: Anexo 3 agrupado por servicio + portador.

## Método de trabajo (cómo avanzamos sin retroceder)

- **Fases.** Todo trabajo nuevo se divide en fases pequeñas registradas en
  `PLAN.md` (por decidir / pendiente / en curso / hecha). Una fase a la vez:
  la siguiente no se abre sin que el usuario valide la anterior.
- **Anti-regresión.** Lo que ya funciona es intocable salvo acuerdo explícito.
  Antes de dar una fase por terminada: probar con los respaldos JSON reales y
  verificar que las pestañas y flujos existentes siguen funcionando. Si algo
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

- Un solo archivo `index.html` (~425 KB), JavaScript vanilla, sin
  dependencias externas. Nada de frameworks, builds ni librerías salvo
  acuerdo explícito con el usuario.
- Persistencia en `localStorage` (clave `geb_hrt_v3`), con exportación e
  importación de respaldos `.json` desde la pestaña Datos.
- 11 pestañas: Hoy, Panel, Inventario, Plan MP, Asignación, Documentos,
  Informes, Correctivos, Reportes, Pendientes y Datos (~183 funciones JS).

## Primera sesión con este archivo (hacer UNA sola vez)

1. Si en la carpeta existe `CLAUDE_anterior.md` (el CLAUDE.md previo del
   proyecto), leerlo y traspasar aquí toda regla o decisión de arquitectura
   que no esté ya en este archivo. Avisar al usuario qué se rescató y
   eliminarlo.
2. Leer `index.html`, `HISTORIAL.md` y `README.md`, y verificar que lo
   descrito aquí coincide con el código real. Si algo difiere, corregir este
   archivo, no el código.
3. Resumir en máximo 10 líneas, sin tecnicismos, en qué está el proyecto y
   cuál es la fase propuesta en `PLAN.md`. Esperar el OK del usuario antes de
   tocar cualquier cosa.
