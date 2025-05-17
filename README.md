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

### Elementos Clave del Diagrama

#### Relaciones Estructurales
1. **Extensión** (`‹‹extend››`):
   - `Cancelar Turno` extiende `Consultar Turno` (relación condicional)
   - *Justificación*: Solo se ejecuta cuando el cliente solicita cancelación activa

2. **Inclusión** (`‹‹include››`):
   - `Generar Código` requiere obligatoriamente `Solicitar Turno`
   - `Notificar Turno` siempre ejecuta `Atender Turno`
   - *Ventaja*: Garantiza integridad del proceso

### Análisis Técnico
**Patrones aplicados**:
1. **Facade** (en `Llamar Turno`):
   - Simplifica interacción con subsistemas complejos (pantallas/audio)
   
2. **Observer**:
   - Notificaciones multicanal se actualizan automáticamente

**Reglas de negocio mapeadas**:
- Secuencia estricta: Generar → Asignar → Notificar
- Priorización configurable por servicio

---

---

## Diagrama de Clases UML
![diagramaClases](https://github.com/user-attachments/assets/1b448a3d-e19e-490e-b8fc-8622e10371b5)


### Patrones Implementados:
1. **Singleton**:
   - `Turno`: Garantiza una única instancia controladora
   - Justificación: Evita conflictos en asignación concurrente

2. **Observer**:
   - Relación entre `Turno` y `SistemaNotificación`
   - Ventaja: Notificaciones en tiempo real sin acoplamiento

3. **Factory Method**:
   - `GeneradorReportes` con implementaciones concretas (PDF, Excel)
   - Beneficio: Extensibilidad para nuevos formatos

4. **Strategy**:
   - Algoritmos intercambiables para `AsignadorTurnos` (FIFO, Prioridad)
   - Impacto: Flexibilidad en políticas de atención

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
