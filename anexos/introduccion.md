# Introducción

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

## Requisitos Funcionales

### RF1: Crear, Editar y Eliminar Proyecto
El sistema debe permitir crear, editar y eliminar proyectos audiovisuales.  
- **Medición**: Cada acción debe completarse en menos de 3 segundos, en el 95% de los intentos.

### RF2: Asociar Etapas a Proyectos
El sistema debe permitir asociar una o más etapas (ej. grabación, edición) a cada proyecto.  
- **Medición**: Cada proyecto debe tener al menos una etapa asignada al momento de su creación.

### RF3: Visualizar Historial de Cambios
El sistema debe permitir que los usuarios visualicen el historial de cambios de un proyecto en tiempo real.  
- **Medición**: El historial debe cargar en menos de 2 segundos y reflejar todos los cambios previos.

### RF4: Subida de Archivos Adjuntos
Los usuarios deben poder subir archivos (videos, imágenes, documentos) asociados a cada etapa del proyecto.  
- **Medición**: El tiempo máximo de subida de archivos no debe superar los 5 minutos por archivo.

### RF5: Agregar Comentarios a Etapas
El sistema debe permitir a los usuarios agregar comentarios a cada etapa de un proyecto.  
- **Medición**: Los comentarios deben ser visibles y creables en menos de 1 segundo tras hacer clic en el campo de texto.


## Requisitos No Funcionales

### RNF1: Alta Disponibilidad
El sistema debe tener una alta disponibilidad (≥ 99% de tiempo operativo).  
- **Medición**: El sistema no debe tener más de 5 horas de inactividad al mes.

### RNF2: Facilidad de Uso
La interfaz debe ser fácil de usar, con un diseño intuitivo.  
- **Medición**: Los usuarios deben completar una tarea básica (crear un proyecto) en menos de 3 clics.

### RNF3: Compatibilidad con Dispositivos Móviles
El sistema debe ser completamente responsivo y compatible con dispositivos móviles.  
- **Medición**: El sistema debe adaptarse a diferentes tamaños de pantalla, sin pérdida de funcionalidad.

### RNF4: Capacidad de Escalabilidad
El sistema debe ser capaz de manejar más de 500 usuarios simultáneos sin perder rendimiento.  
- **Medición**: El tiempo de respuesta para cualquier operación no debe superar los 3 segundos bajo carga máxima de usuarios.

### RNF5: Seguridad y Autenticación
El sistema debe garantizar autenticación y autorización de usuarios.  
- **Medición**: El sistema debe cumplir con los estándares de seguridad **OAuth 2.0** y asegurar contraseñas con un nivel de encriptación de **256 bits**.

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

<img width="530" height="488" alt="diseño 1" src="https://github.com/user-attachments/assets/4d994811-6944-40dc-b0e6-89cfa2fb3544" />

