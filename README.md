# Sistema de Gestión de Turnos - Documentación Técnica

## Descripción General
El Sistema de Gestión de Turnos es una solución integral para la administración automatizada de flujos de atención en entornos corporativos. Proporciona:
- Asignación inteligente de turnos basada en disponibilidad de empleados
- Notificaciones multicanal (visuales y auditivas)
- Generación de reportes estadísticos
- Configuración flexible de servicios y prioridades

**Objetivo principal**: Optimizar el tiempo de espera y mejorar la experiencia del cliente mediante un flujo automatizado de atención.

---

## Diagrama de Casos de Uso
![diagramaCaso](https://github.com/user-attachments/assets/f722ed9b-a009-4b0d-b578-29d74703a129)

### Flujo Principal
1. **Selección de turno**: El sistema identifica automáticamente el siguiente turno en cola
2. **Asignación**: Vincula el turno al primer empleado disponible
3. **Notificación**: Dispara alertas simultáneas a:
   - Sistema de pantallas (visual)
   - Sistema de audio (auditiva)
#### Relaciones Estructurales
1. **Extensión** (`‹‹extend››`):
   - `Cancelar Turno` extiende `Consultar Turno` (relación condicional)
   - *Justificación*: Solo se ejecuta cuando el cliente solicita cancelación activa

2. **Inclusión** (`‹‹include››`):
   - `Generar Código` requiere obligatoriamente `Solicitar Turno`
   - `Notificar Turno` siempre ejecuta `Atender Turno`
   - *Ventaja*: Garantiza integridad del proceso
     

---

---

## Diagrama de Clases UML
![diagramaClases](https://github.com/user-attachments/assets/1b448a3d-e19e-490e-b8fc-8622e10371b5)

### Patrones Implementados:

1. **Singleton**:
   - `TurnoRepositoryImpl`: Garantiza única instancia del repositorio
   - Justificación: Centraliza el acceso a datos de turnos y previene inconsistencia en entornos concurrentes

2. **Strategy**:
   - **FIFOStrategy**:
     - Implementa cola tradicional (first-in-first-out)
     - `asignaTurno()` devuelve el empleado más antiguo disponible
     - Ideal para servicios estándar sin prioridades
   
   - **RoundRobinStrategy**:
     - Asignación rotativa equitativa entre empleados
     - `asignaTurno()` cicla mediante índice rotatorio
     - Optimiza distribución de carga laboral

3. **Factory Method**:
   - `TurnoFactory` con implementaciones:
     - `TurnoNormalFactory`: Genera códigos con prefijo "N-"
     - `TurnoPrioritarioFactory`: Genera códigos con prefijo "P-"
   - Beneficio: Centraliza lógica de creación según tipo de turno


---

## Diagrama de Implementación
![implementacionUML](https://github.com/user-attachments/assets/7b85b1c8-2c13-4465-b49a-0b894f1a05cc)


### Decisiones Técnicas Clave:
1. **Arquitectura**:
   - Modelo en 3 capas (Presentación, Lógica, Datos)
   - Tecnologías: Java Spring Boot (backend), React (frontend)

2. **Integraciones**:
   - API REST para sistemas externos (pantallas/audio)
   - Protocolo WebSocket para notificaciones en tiempo real

3. **Persistencia**:
   - PostgreSQL para datos transaccionales
   - Redis para caché de turnos activos
  

## Reflexiones Finales de Modelado

### Lecciones Clave Aprendidas

1. **Efectividad de Patrones**:
   - El patrón **Strategy** permitió cambiar políticas de asignación sin modificar código base
   - **Factory Method** simplificó la creación de turnos con diferentes lógicas de generación
   - **Singleton** aseguró consistencia en el repositorio de turnos

2. **Validación Práctica**:
   - El modelo inicial requirió ajustes para:
     - Manejar casos de empleados no disponibles
     - Gestionar turnos prioritarios vs regulares
   - La separación entre generación y asignación demostró ser acertada

3. **Resultados Concretos**:
   - Reducción de tiempos de asignación
   - Mayor flexibilidad para agregar nuevos tipos de turnos
   - Centralización del manejo de estados
  
### Análisis Crítico

**Fortalezas**:
✔ Sistema altamente modular y mantenible  
✔ Fácil extensión para nuevos requerimientos  
✔ Buen desempeño en condiciones normales  

**Debilidades**:
✖ Complejidad inicial en configuración  
✖ Falta de manejo de errores en flujos alternativos  
