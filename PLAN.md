# PLAN.md — Fases del proyecto Sistema MP 2026

Estados: **por decidir → pendiente → en curso → hecha** (o descartada).
Una fase a la vez; se cierra solo con validación del usuario.
Al cerrar una fase: actualizar este archivo, anotar decisiones nuevas en
`CLAUDE.md` y hacer commit.

## En curso (esperando validación del usuario)

1. **Persistencia local (v17)**: el programa guardaba solo dentro de
   claude.ai; ahora también guarda en el navegador al usarlo como archivo
   local. Probar: registrar una MP, cerrar el navegador, reabrir el archivo
   y verificar que el registro sigue.

## Propuestas esperando OK (hoja de ruta heredada del README)

2. **Fase 3a — Confiabilidad**: hoja "Para actualizar Excel" en la
   exportación + aviso de respaldo al cerrar la jornada.
3. **Fase 4 — Informes**: cumplimiento mensual por servicio/ejecutor para
   jefatura.
4. **Fase 3c — Pendientes**: por definir con el usuario.

## Por decidir (preguntar el "para qué" antes de partir)

5. **Papeleo de anexos** (del levantamiento del flujo real 11-06): imprimir
   protocolo MP (Anexo 2), reporte de reprogramación (Anexo 3 — ¿hoja por
   equipo o grupal por servicio? pregunta abierta) y retiro (Anexo 4), con
   portador del papel y semáforo de papel pendiente.
6. **Embeber la librería de Excel** para que importar/exportar funcione sin
   internet (el archivo crecería ~1 MB).
7. % operativo por familia de equipos (disponibilidad).
8. Imprimir los protocolos del mes por técnico desde Plan MP.

## Hechas

- 12-06-2026 · **Decisión de rumbo**: se continúa con Sistema MP 2026; GEB
  (el programa anterior de 11 pestañas) queda congelado. Repositorio GitHub
  creado con programa, documentación y respaldos reales (`respaldos/`).
- 11-06-2026 · v1–v16: Fase 1 Preventivo ✔ · Fase 2 Correctivo ✔ (pendiente
  validación con casos reales) · Fase 3b Ficha técnica ✔ · documentación
  (README, HISTORIAL). Ver `HISTORIAL.md`.
- 11-06-2026 · Levantamiento del flujo real del proceso interno (documentado
  en `CLAUDE.md`): ciclos, orden programa → carpeta → Excel, reprogramación
  implícita, los 3 papeles, portador.
