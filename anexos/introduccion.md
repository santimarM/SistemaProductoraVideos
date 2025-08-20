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

## Requisitos Iniciales del Sistema

### Funcionales
1. Registrar clientes y proyectos.  
2. Asignar empleados y roles a proyectos.  
3. Gestionar recursos (equipos, locaciones).  
4. Definir y monitorear tareas de producción.  
5. Generar reportes de avance y costos.  

### No Funcionales
1. Interfaz intuitiva y fácil de usar.  
2. Seguridad en los datos (autenticación y backups).  
3. Alta disponibilidad del sistema.  
4. Escalabilidad para múltiples proyectos en paralelo.  
5. Integración futura con plataformas de streaming y almacenamiento en la nube.  

---

## Casos de Uso

### Caso de Uso 1 – Registrar Proyecto
- **Actor:** Productor  
- **Descripción:** Permite crear un nuevo proyecto de video asignando cliente, equipo y presupuesto.  
- **Flujo Principal:**  
  1. El productor inicia sesión.  
  2. Selecciona "Registrar proyecto".  
  3. Ingresa datos (nombre, cliente, fecha, presupuesto).  
  4. El sistema valida la información.  
  5. El sistema confirma el registro.  
- **Precondiciones:** Cliente registrado.  
- **Postcondiciones:** Proyecto registrado en el sistema.  

### Caso de Uso 2 – Asignar Empleados
- **Actor:** Coordinador  
- **Descripción:** Asigna empleados y roles a un proyecto.  
- **Flujo Principal:**  
  1. Coordinador selecciona proyecto.  
  2. Selecciona empleados disponibles.  
  3. Asigna roles.  
  4. El sistema registra asignación.  
  5. Se confirma la operación.  
- **Precondiciones:** Proyecto registrado.  
- **Postcondiciones:** Empleados asignados al proyecto.  

### Caso de Uso 3 – Gestionar Recursos
- **Actor:** Coordinador  
- **Descripción:** Permite reservar equipos o locaciones.  
- **Flujo Principal:**  
  1. Coordinador abre gestión de recursos.  
  2. Selecciona recurso y fecha.  
  3. El sistema verifica disponibilidad.  
  4. Registra la reserva.  
  5. Confirma asignación.  
- **Precondiciones:** Proyecto activo.  
- **Postcondiciones:** Recursos asignados al proyecto.  

### Caso de Uso 4 – Monitorear Avance
- **Actor:** Productor  
- **Descripción:** Consultar estado de las tareas de un proyecto.  
- **Flujo Principal:**  
  1. Productor selecciona proyecto.  
  2. Abre panel de avance.  
  3. El sistema muestra lista de tareas.  
  4. Muestra estado (pendiente, en curso, finalizada).  
  5. Productor analiza y guarda reporte.  
- **Precondiciones:** Proyecto registrado.  
- **Postcondiciones:** Avance visualizado.  

### Caso de Uso 5 – Generar Reporte de Costos
- **Actor:** Administrador  
- **Descripción:** Generar reporte financiero del proyecto.  
- **Flujo Principal:**  
  1. Administrador selecciona proyecto.  
  2. Solicita reporte de costos.  
  3. El sistema recopila gastos y presupuesto.  
  4. Genera reporte en PDF.  
  5. Muestra al usuario.  
- **Precondiciones:** Proyecto con gastos registrados.  
- **Postcondiciones:** Reporte generado.  

---

## Boceto Inicial de Clases
![Diagrama de Clases] 
<img width="530" height="488" alt="diseño 1" src="https://github.com/user-attachments/assets/4d994811-6944-40dc-b0e6-89cfa2fb3544" />

