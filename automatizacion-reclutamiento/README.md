# Sistema de reclutamiento — estado actual y automatización

Última actualización: 2026-09-18

## 1. Dónde vive todo

**Directorio de Candidatos (el sistema único, en vivo):**
https://claude.ai/artifact/Pz1StP8f88L6j2VdCmkTm9

- 15,431 candidatos base (consolidado de las 26 bases originales).
- 1,262 con actividad real: reclutadora asignada, CV, estado y/o comentarios (extraídos de las carpetas "Base de Datos Team Spot Hunting" de Drive: Ema, Fernanda, Ignacia, Mabel, Paula, Valeria, Yasna, septiembre + agosto 2026 completos).
- 15 casos ambiguos (mismo nombre, 2+ candidatos posibles) sin tocar — ver `Candidatos_Ambiguos_Revisar.xlsx` en esta carpeta.

**Google Drive** ("Base de Datos Team Spot Hunting"): se deja tal cual, como respaldo. No se modifica su contenido, solo se lee. Las carpetas de fecha ya quedaron renombradas a formato `YYYY-MM-DD` para todas las reclutadoras (excepto los contenedores "Agosto 2026", que se dejaron intactos por tener otra estructura).

Esta carpeta (`automatizacion-reclutamiento/`) es el **prototipo/bitácora**: aquí quedan los scripts, reportes y este documento — no reemplaza el directorio, lo documenta y lo alimenta.

## 2. Automatización YA activa (corriendo sola)

### Carpeta diaria por reclutadora
- **Qué hace:** cada mañana (lunes a viernes, 8am hora CDMX) revisa si ya existe la carpeta de Drive con la fecha de hoy dentro de cada una de las 7 carpetas de reclutadora. Si no existe, la crea.
- **Por qué:** para que nadie del equipo (ni tú) tenga que crearla a mano cada día.
- **Estado:** funcionando — se probó el 17 y 18 de septiembre, creó las carpetas faltantes de Mabel, Yasna, Ignacia y Ema sin intervención.
- Rutina: `Crear carpeta diaria CVs por reclutadora` (trigger_id `trig_01WDE1rDXW4KCm375pdSoXzy`).

## 3. Automatización DISEÑADA pero pendiente de tu autorización

### Reparto diario de 35 leads por reclutadora
- **Lógica ya definida:** cada mañana laboral, tomar los siguientes 245 candidatos sin reclutadora asignada (35 × 7 personas), priorizando CDMX/Zona Metro primero, luego Mexicali, luego el resto — mismo criterio que se usó desde el inicio. A cada uno se le asigna `reclutador_asignado`, estado "nueva", y una nota de "asignado automáticamente el <fecha>".
- **Por qué no está activa todavía:** el sistema de permisos bloqueó la creación de esta rutina automática porque escribe en la base de datos compartida sin supervisión humana en cada corrida (a diferencia de crear una carpeta vacía, esto mueve cientos de registros todos los días). Necesita tu autorización explícita.
- **Cómo activarla:** dímelo y la creo; o si prefieres control total, puedo dejarte instrucciones para crearla tú mismo desde el panel de Rutinas en claude.ai (ahí puedes revisar y aprobar el permiso de escritura tú mismo al crearla).
- **Cuánto dura el stock:** con ~14,169 candidatos base aún sin asignar, a 245/día alcanza para ≈ 58 días hábiles (~11-12 semanas) antes de agotarse. La rutina está diseñada para avisarte automáticamente cuando el stock esté por acabarse.

### Reparto de CVs (documentos, no solo datos)
- Distinto de los leads: un CV solo puede "repartirse" si existe. Ya sabemos que sí hay volumen real (miles en las carpetas de Drive). La automatización de "20 CV por persona por día" tiene sentido una vez definamos: ¿son CVs que la reclutadora sube ella misma (no se reparten, se registran), o quieres que el sistema le asigne CVs de candidatos que otra persona ya subió? Aclarando esto, la implemento en la misma rutina.

## 4. Limpieza pendiente (no automatizable sin cambio de permisos)

**13 CVs duplicados** identificados en Drive (mismo archivo subido 2+ veces el mismo día) no se pudieron enviar a la papelera: el Drive pertenece a la cuenta `agustin.spothuntingchile@gmail.com` y el acceso compartido actual no incluye permiso de borrado sobre archivos ajenos.

**Dos formas de resolverlo:**
1. Tú (o esa cuenta) los borra manualmente — lista abajo.
2. Le das a esta sesión rol de "Editor con permiso de eliminar" sobre esa carpeta de Drive, y lo hago yo automáticamente, incluyendo cualquier duplicado futuro.

Lista de los 13 duplicados (carpeta / archivo a conservar / archivo a borrar):
| Reclutadora | Carpeta | Conservar | Borrar |
|---|---|---|---|
| Paula | 2026-09-17 | `EDUARDO CV.pdf` | `EDUARDO CV (1).pdf` |
| Fernanda | 2026-09-16 | (original) | `Nayeli_Jimenez_Ochoa (1).pdf` |
| Paula | 2026-09-14 | `CV_Jose_Alberto_Montes_Vargas (1).pdf` | `CV_Jose_Alberto_Montes_Vargas (2).pdf` |
| Paula | 2026-09-10 | (original) | `Jackeline_Villa (1).pdf` |
| Paula | 2026-09-10 | (original) | `eva_maría_lozano (1).pdf` |
| Paula | 2026-09-09 | (original) | `CV_Alan_Gonzalez_Emojis (1).pdf` |
| Paula | 2026-09-07 | (original) | `cv actualizado.pdf 2026 (1).pdf` |
| Paula | 2026-09-02 | (original) | `CV (4).pdf` |
| Paula | 2026-09-02 | (original) | `CV fin 369 (3).pdf` |
| Paula | 2026-09-01 | `cv_Itzel_Hidalgo_Muñoz 2026.pdf` | `(1)`, `(2)`, `(3)`, `(4)` — 4 copias |

## 5. Casos ambiguos pendientes de tu revisión

Ver `Candidatos_Ambiguos_Revisar.xlsx` — 15 nombres de CV que coinciden con 2 o más candidatos distintos en la base de 15,431. No se les asignó nada para no adivinar. Marca en la columna "Candidato posible - ID" cuál es el correcto (o si son personas distintas, indícalo) y lo cargo.

## 6. Cómo usar el sistema, resumen rápido

| Quién | Qué hace |
|---|---|
| Reclutadora | Abre el link del directorio, contacta candidatos asignados, actualiza estado y CV en vivo |
| Agustín (admin) | Filtra por estado/región/reclutadora para ver el avance; resuelve ambiguos y flags; decide cuándo se reabastece el stock de leads |
| Sistema | Crea la carpeta del día solo; (pendiente de tu ok) reparte 35 leads/día por persona automáticamente |

## 7. Siguientes decisiones que necesito de ti

1. ¿Activo la rutina de reparto diario de 35 leads/persona? (sí/no, o ajustar el número)
2. ¿Cómo defines el reparto de CVs — se suben o se asignan?
3. ¿Me das permiso de eliminar en Drive para los duplicados, o los borras tú con la lista de arriba?
4. Revisar y devolver `Candidatos_Ambiguos_Revisar.xlsx`
