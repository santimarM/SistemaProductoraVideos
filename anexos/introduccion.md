# Anexo - Introducción al Diseño Orientado a Objetos

## Paradigma Orientado a Objetos
El paradigma orientado a objetos (POO) se centra en modelar el software a partir de **objetos** que representan entidades del mundo real, incluyendo atributos (datos) y métodos (comportamiento).  
Es importante porque permite modularidad, reutilización, escalabilidad y mejor mantenimiento del sistema.

---

## Los Cuatro Fundamentos de POO
1. **Abstracción:** Representar solo lo esencial.  
   - Ejemplo: Clase `Proyecto` abstrae la complejidad y muestra solo atributos como nombre, cliente y estado.

2. **Encapsulamiento:** Proteger los datos internos y exponer métodos controlados.  
   - Ejemplo: Clase `Empleado` expone métodos `asignarRol()` sin exponer directamente sus atributos privados.

3. **Herencia:** Permite crear nuevas clases basadas en otras.  
   - Ejemplo: Clase `Empleado` → subclases `Productor`, `Editor`.

4. **Polimorfismo:** Permite que distintas clases respondan a un mismo mensaje.  
   - Ejemplo: Método `calcularCosto()` puede comportarse distinto en `ProyectoCorto` y `ProyectoDocumental`.

---

# Requisitos Iniciales del Sistema

## Requisitos Funcionales (RF)

RF1 — ABM de Clientes
Debe: crear, editar y eliminar clientes con: nombre, empresa, teléfono, email, país, provincia, localidad, CP y domicilio.
Criterios: valida formato de email y teléfono; todos los obligatorios completos; toda alta/modificación queda auditada.

RF2 — ABM de Proyectos
Debe: crear/editar/eliminar proyectos con: nombre (único por cliente), tipo ∈ {Publicidad, VideoClip, Institucional}, fechas inicio/fin, responsable general y estado ∈ {En curso, Pausado, Terminado}.
Criterios: no permite fin < inicio; estado por defecto = En curso (o Pausado si se indica); operaciones auditadas.

RF3 — ABM de Etapas
Debe: por proyecto, crear/editar/eliminar etapas con: nombre, tipo ∈ {Estándar, Personalizada}, fechas estimadas, responsable, estado ∈ {Pendiente, En curso, Finalizada}, prioridad (entera) y comentarios.
Criterios: no permite fechas estimadas fuera del rango del proyecto; cambios de estado y datos quedan auditados.

RF4 — Workflow de Estado de Etapa
Debe: validar transiciones: Pendiente → En curso → Finalizada y (opcional) En curso ↔ Pausada.
Criterios: rechaza transiciones inválidas con mensaje; registra {de, a, usuario, fecha} en auditoría.

RF5 — Asignación de Responsables y Roles
Debe: asignar responsables y roles a proyectos, etapas y tareas, guardando historial de vigencia.
Criterios: cada etapa tiene exactamente 1 responsable activo; no se permite más de un responsable activo simultáneo; historial consultable.

RF6 — Gestión de Recursos y Reservas
Debe: administrar recursos (equipos, locaciones, usuarios/roles) y reservarlos por etapa con rango de fechas.
Criterios: no se permite superposición de reservas del mismo recurso; estados de reserva ∈ {Reservada, Cancelada}.

RF7 — Tareas de Producción por Etapa
Debe: definir tareas con descripción, estado y vencimiento vinculadas a una etapa.
Criterios: estados mínimos ∈ {Pendiente, En curso, Hecha}; no se puede cerrar una etapa con tareas Pendiente/En curso.

RF8 — Comentarios y Enlaces
Debe: registrar comentarios y adjuntar links (Drive/Vimeo/otros) en proyecto y/o etapa.
Criterios: URL http/https válida; guarda autor y fecha; visibilidad según permisos.

RF9 — Tablero, Filtros y Búsqueda
Debe: mostrar tablero con filtros por estado, responsable, tipo y cliente; búsqueda por nombre de proyecto, cliente, responsable y etiquetas.
Criterios: filtros impactan conteos y listados; búsqueda por prefijo y palabra completa.

RF10 — KPIs y Reportes
Debe: calcular y mostrar, con exportación CSV/PDF:

Proyectos Activos/En riesgo/Retrasados (En riesgo si desviación de fecha estimada > 20% o faltan ≤ 3 días y hay tareas abiertas; Retrasado si vencida fecha estimada de fin).

Total vs estimado por proyecto y por etapa (tiempo real vs plan).

Incidencias/cambios por proyecto.

Volumen mensual por tipo de proyecto y por cliente.
Criterios: resultados reproducibles con la misma data y filtros.

RF11 — Notificaciones Automáticas (Email/WhatsApp)
Debe: notificar al crear/finalizar etapas, al asignar/cambiar responsables y al detectar retrasos.
Criterios: latencia evento→mensaje ≤ 60 s p95; log de resultado (enviado/error/reintento) y destinatarios.


## Requisitos No Funcionales (RNF)

RNF1 — Disponibilidad (Producto)
Debe: disponibilidad mensual ≥ 99,5% en horario hábil (L–V 08:30–17:30).
Métrica: indisponibilidad acumulada ≤ 3h 39m/mes en ese horario.

RNF2 — Rendimiento (Producto)
Debe:

Listado con filtros: < 2 s p90 con 5k proyectos / 50k etapas.

Dashboard/KPIs: < 3 s p90.

Búsqueda simple: < 1,5 s p90.
Métrica: tiempos de respuesta medidos en entorno de prueba con dataset objetivo.

RNF3 — Latencia de Notificaciones (Producto)
Debe: pipeline evento→notificación ≤ 60 s p95 (ver RF11).
Métrica: distribución de latencias por tipo de evento.

RNF4 — Usabilidad (Producto)
Debe: tras 4 h de capacitación, usuarios internos ejecutan tareas típicas con ≤ 2 errores/hora; encuesta SUS ≥ 75 a la semana.
Métrica: pruebas de usabilidad y cuestionario SUS.

RNF5 — Seguridad y Acceso (Organización)
Debe: autenticación nominativa y control de acceso por rol (Admin/Responsable/Lector); toda operación crítica auditada.
Métrica: registro de auditoría completo y revisiones periódicas.

RNF6 — Respaldo y Recuperación (Organización)
Debe: RPO ≤ 24 h y RTO ≤ 4 h para la base de datos; pruebas de restauración trimestrales documentadas.
Métrica: evidencia de última copia y último restore exitoso.

RNF7 — Escalabilidad (Producto)
Debe: soportar 200 usuarios concurrentes manteniendo métricas de rendimiento; dataset objetivo 10k proyectos / 100k etapas sin degradación p90.
Métrica: prueba de carga.

RNF8 — Portabilidad/Compatibilidad (Producto)
Debe: web “desktop-first” y responsive; navegadores soportados = últimas 2 versiones de Chrome, Edge y Firefox; móviles con ancho ≥ 360 px.
Métrica: matriz de compatibilidad en QA.

RNF9 — Mantenibilidad (Organización)
Debe: arquitectura modular (dominio/servicios/infra), logging y monitoreo básico; tiempo medio de corrección de bug severidad media ≤ 3 días hábiles.
Métrica: KPIs de mantenimiento y registros de incidentes.

---

## Casos de Uso

### Caso de Uso 1 – Crear proyecto
- **Actor:** Admin  
- **Descripción:** Crea un nuevo proyecto con sus datos iniciales.  
- **Flujo Principal:**  
  1. Admin inicia sesión.  
  2. Selecciona **“Crear proyecto”**.  
  3. Ingresa datos (nombre, tipo, cliente, fechas, responsable general, observaciones).  
  4. El sistema valida la información.  
  5. El sistema registra el proyecto y confirma.  
- **Precondiciones:** Cliente existente (o alta de cliente previa).  
- **Postcondiciones:** Proyecto creado en estado **“En curso”** (o **“Pausado”** si se define así).

---

### Caso de Uso 2 – Editar / Pausar / Reanudar / Cerrar proyecto
- **Actor:** Admin  
- **Descripción:** Permite actualizar datos del proyecto y cambiar su estado.  
- **Flujo Principal:**  
  1. Admin busca y abre el proyecto.  
  2. Selecciona **Editar** y modifica campos necesarios.  
  3. (Opcional) Cambia estado a **Pausado**, **En curso** (reanudar) o **Cerrado**.  
  4. El sistema valida y guarda cambios.  
  5. El sistema confirma la operación.  
- **Precondiciones:** Proyecto existente; permisos de Admin.  
- **Postcondiciones:** Proyecto actualizado; si se cambió el estado, se registra en historial.

---

### Caso de Uso 3 – Configurar etapas (agregar/editar/eliminar/personalizadas)
- **Actor:** Admin  
- **Descripción:** Administra el conjunto de etapas del proyecto (estándar o personalizadas).  
- **Flujo Principal:**  
  1. Admin abre el proyecto y entra a **Etapas**.  
  2. Agrega/edita/elimina etapas (nombre, fechas, prioridad, responsable).  
  3. El sistema valida dependencias/fechas.  
  4. El sistema guarda la configuración y confirma.  
- **Precondiciones:** Proyecto en estado **En curso** o **Pausado**.  
- **Postcondiciones:** Etapas registradas/actualizadas; se registra historial.

---

### Caso de Uso 4 – Asignar responsable de etapa
- **Actor:** Admin  
- **Descripción:** Asigna o reasigna un responsable a una etapa específica.  
- **Flujo Principal:**  
  1. Admin abre el proyecto → pestaña **Etapas**.  
  2. Selecciona una etapa y elige **Asignar responsable**.  
  3. Busca y selecciona al usuario.  
  4. El sistema guarda la asignación y (opcional) notifica al usuario.  
- **Precondiciones:** Etapa existente; usuario activo en el sistema.  
- **Postcondiciones:** Etapa con responsable asignado; registro en historial y cola de notificaciones.

---

### Caso de Uso 5 – Cambiar estado de etapa
- **Actor:** Responsable de Etapa  
- **Descripción:** Permite marcar la etapa como **Pendiente / En curso / Finalizada** respetando transiciones válidas.  
- **Flujo Principal:**  
  1. Responsable abre **Mis proyectos / Mis etapas**.  
  2. Selecciona la etapa y pulsa **Cambiar estado**.  
  3. Elige el nuevo estado (p. ej., de *Pendiente* a *En curso*).  
  4. El sistema valida la transición, guarda y (opcional) dispara notificación.  
- **Precondiciones:** Etapa asignada al responsable; proyecto no **Cerrado**.  
- **Postcondiciones:** Estado de la etapa actualizado y auditado.

---

### Caso de Uso 6 – Registrar comentario/observación
- **Actor:** Responsable de Etapa / Asistente  
- **Descripción:** Agrega comentarios u observaciones en una etapa o proyecto.  
- **Flujo Principal:**  
  1. Usuario abre el proyecto/etapa.  
  2. Selecciona **Agregar comentario**.  
  3. Escribe el texto y guarda.  
  4. El sistema registra el comentario con fecha y autor.  
- **Precondiciones:** Acceso al proyecto/etapa.  
- **Postcondiciones:** Comentario agregado al historial del proyecto/etapa.

---

### Caso de Uso 7 – Adjuntar link por etapa/proyecto
- **Actor:** Responsable de Etapa  
- **Descripción:** Adjunta enlaces (Drive/Vimeo/otros) asociados a una etapa o al proyecto.  
- **Flujo Principal:**  
  1. Responsable abre el proyecto/etapa.  
  2. Selecciona **Adjuntar link**.  
  3. Ingresa URL y descripción/tipo.  
  4. El sistema valida el formato y guarda.  
- **Precondiciones:** Proyecto/etapa existente; permisos del responsable.  
- **Postcondiciones:** Link asociado y visible según permisos.

---

### Caso de Uso 8 – Notificar evento (automático)
- **Actor:** **Sistema**  
- **Descripción:** Envía notificaciones por Email/WhatsApp ante eventos configurados.  
- **Flujo Principal:**  
  1. Ocurre un evento (p. ej., etapa finalizada, asignación de responsable, retraso).  
  2. El sistema genera el mensaje con plantilla y destinatarios.  
  3. Envía por el canal configurado (SMTP / API WhatsApp).  
  4. Registra resultado (enviado, error, reintento) en el log/historial.  
- **Precondiciones:** Canales y destinatarios configurados; conectividad disponible.  
- **Postcondiciones:** Notificación enviada o en cola de reintentos; traza registrada.

---

### Caso de Uso 9 – Ver tablero y filtrar
- **Actor:** **Todos** (Admin, Responsable, Asistente)  
- **Descripción:** Visualiza el tablero de proyectos y aplica filtros.  
- **Flujo Principal:**  
  1. Usuario abre **Tablero**.  
  2. Aplica filtros (estado, responsable, tipo, cliente).  
  3. El sistema muestra la lista y contadores (activos, en riesgo, retrasados).  
  4. Usuario entra al detalle si lo necesita.  
- **Precondiciones:** Usuario autenticado.  
- **Postcondiciones:** Información visualizada; no hay cambios persistentes.

---

### Caso de Uso 10 – Gestionar clientes y contactos
- **Actor:** Admin  
- **Descripción:** ABM de clientes/contacts asociados a proyectos.  
- **Flujo Principal:**  
  1. Admin abre **Clientes**.  
  2. Crea/edita/elimina cliente o contacto.  
  3. El sistema valida datos (email/teléfono) y guarda.  
  4. (Opcional) Asocia cliente a un proyecto.  
- **Precondiciones:** Permisos de Admin.  
- **Postcondiciones:** Cliente/Contacto registrado o actualizado.

---

### Caso de Uso 11 – Consultar métricas y exportar
- **Actor:** Admin  
- **Descripción:** Consulta indicadores y exporta reportes (PDF/CSV).  
- **Flujo Principal:**  
  1. Admin abre **Métricas/Reportes**.  
  2. Selecciona rango/filtros (cliente, tipo, fechas).  
  3. El sistema calcula y muestra: tiempos vs estimado, incidencias, volumen mensual, etc.  
  4. Admin elige **Exportar** (PDF/CSV).  
  5. El sistema genera y descarga el archivo.  
- **Precondiciones:** Datos existentes en el período.  
- **Postcondiciones:** Reporte visualizado y/o exportado.

---

### Caso de Uso 12 – Clonar proyecto / Versionar entrega
- **Actor:** Admin  
- **Descripción:** Crea un nuevo proyecto a partir de uno existente o registra nuevas versiones de entregas.  
- **Flujo Principal:**  
  1. Admin abre un proyecto base.  
  2. Selecciona **Clonar proyecto** (y define qué copiar: etapas, responsables, fechas).  
  3. El sistema crea el nuevo proyecto y permite ajustes.  
  4. (Para versionado) Admin agrega **nueva versión de entrega** con notas.  
  5. El sistema registra la versión y mantiene el historial.  
- **Precondiciones:** Proyecto base existente.  
- **Postcondiciones:** Nuevo proyecto clonado y/o entrega versionada con traza.

---

### Caso de Uso 13 – Aprobar etapa
- **Actor:** Cliente  
- **Descripción:** Permite que el cliente apruebe o rechace una etapa entregada.  
- **Flujo Principal:**  
  1. Cliente accede al enlace/portal del proyecto.  
  2. Visualiza el entregable y comentarios.  
  3. Elige **Aprobar** o **Solicitar cambios** (ingresando observaciones).  
  4. El sistema registra la decisión y notifica al equipo.  
- **Precondiciones:** Cliente habilitado; etapa en estado **“Pendiente de aprobación”**.  
- **Postcondiciones:** Etapa aprobada (pasa a **Finalizada**) o vuelve a **En curso** con observaciones.

---

## Boceto Inicial de Clases

<img width="530" height="488" alt="diseño 1" src="https://github.com/santimarM/SistemaProductoraVideos/blob/feature/analista-requerimientos-add-introduccion-md/diagramas/01-boceto-inicial.png" />
