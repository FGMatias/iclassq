# Glosario del Dominio - ICLASSQ

Diccionario oficial del negocio. Define con precisión el significado
de cada término desde la perspectiva del negocio, no del código.

> **Regla de oro:** si un término aparece en un caso de uso, una user story,
> un diagrama, o un nombre de clase, su definición debe estar aquí.

---

## Índice

- [Actores](#actores)
- [Entidades de negocio](#entidades-de-negocio)
- [Procesos](#procesos)
- [Estados](#estados)
- [Configuración](#configuración)
- [Reglas de negocio](#reglas-de-negocio)

---

## Actores

<!-- términos aquí -->

## Entidades de negocio

### Área

**Definición**
Espacio físico delimitado dentro de una Sucursal que agrupa Ventanillas y Servicios relacionados entre sí. Representa una división operativa real de la Sucursal donde trabajan asesores con funciones específicas.

Ejemplos: Plataforma, Caja, Siniestro, Kallpa.

**Sinónimos / Alias**
Ninguno en el sistema actual.

**Lo que NO es**

- No es un departamento administrativo de la empresa, aunque puede coincidir en nombre.
- No es un Puesto de Atención ni una Ventanilla.
- No es un Servicio.

**Relaciones**

- Pertenece a una Sucursal.
- Contiene una o más Ventanillas físicas.
- Contiene uno o más Servicios que se atienden en ella.
- Los Usuarios (asesores) están asociados a un Área.

**Reglas de negocio**

- Un Área debe tener al menos una Ventanilla y un Servicio activo para poder operar.
- Los Servicios y Ventanillas de un Área no pueden ser usados por Puestos de Atención de otra Área dentro de la misma Sucursal.

**Contexto**

- Administración: donde se crea y configura
- Reportes: unidad de agrupacion para análisis por área

---

### Dispositivo

**Definición**
Equipo físico (computadora, tablet, pantalla o TV) registrado en el sistema que opera de forma autónoma como punto de interacción con el sistema. A diferencia de un Usuario humano, un Dispositivo no representa una persona sino una máquina con una función fija dentro de la Sucursal.

**Tipos de Dispositivo:**

- **Kiosko:** punto de emisión de tickets para el cliente.
- **Monitor:** pantalla pública que muestra los tickets llamados en tiempo real.

**Sinónimos / Alias**
En el sistema legacy los dispositivos eran tratados como usuarios dentro de la misma tabla. En el nuevo sistema son una entidad separada.

**Lo que NO es**

- No es un Usuario humano. No tiene DNI, no tiene turno, no tiene ausencias.
- No es una Ventanilla. La Ventanilla es el módulo físico de atención del asesor, no un dispositivo del sistema.

**Relaciones**

- Pertenece a un Puesto de Atención.
- Pertenece a una Sucursal.
- Su IP se registra automáticamente en cada login.
- Tiene una configuración específica según su tipo.

**Configuración por tipo:**

Kiosko:

- Pide documento de identidad: sí/no
- Imprime ticket: sí/no
- Confirma impresión: sí/no

Monitor:

- Tipo de diseño/layout visual
- Servicios que muestra (heredados del Puesto de Atención)

**Reglas de negocio**

- Un Dispositivo tiene un tipo fijo (Kiosko o Monitor). No puede cambiar de tipo sin intervención del administrador.
- Un Dispositivo está asociado a un único Puesto de Atención.
- La IP del Dispositivo se registra automáticamente al hacer login. No se ingresa manualmente.

**Contexto**

- Kiosko: opera como punto de emisión de tickets
- Monitor: opera como pantalla pública de visualización
- Administración: donde se crea, configura y gestiona

---

### Servicio

_(nombre anterior: Grupo)_

**Definición**
Opción de atención que el cliente selecciona al llegar a la sucursal para indicar el motivo de su visita. Representa el tipo de gestión que el cliente necesita realizar. Cada Servicio tiene su propia cola de atención independiente y puede tener configurado
un algoritmo de cola específico.

Ejemplos: Trámite, Venta, Caja, Informes, Consulta Médica

**Sinónimos / Alias**
Grupo (legacy). En algunos clientes del sector salud: especialidad

**Lo que NO es**

- No es el tipo de cliente, eso lo define la Categoría.
- No es una ventanilla ni un punto de atención físico.
- No es un departamente o área de la empresa, aunque puede coincidir en nombre

**Relaciones**

- Pertenece a una Sucursal.
- Contiene una o más Categorias.
- Tiene asignado un Algoritmo de Cola.
- Es atendido por un o más Ventanillas a través del Rol de Equipo.
- Genera Tickets cuando un cliente lo selecciona.

**Reglas de negocio**

- Un Servicio debe tener al menos una Categoría activa para poder emitir tickets.
- Un Servicio tiene un prefijo único dentro de la sucursal que forma parte del código del Ticket.
- Un Servicio puede estar activo o inactivo. Si está inactivo no aparece en el kiosko.

**Contexto**

- Kiosko: el cliente lo selecciona para iniciar la emisión de su Ticket.
- Administración: donde se crea y configura.
- Reportes: unidad de análisis principal (atenciones por servicio, tiempos por servicio).

---

### Categoria

_(nombre anterior: Subgrupo)_

**Definición**
Clasificación dentro de un Servicio que determina el tipo o la prioridad de atención del cliente. Permite diferencias a clientes que vienen por el mismo Servicio pero requieren un tratamiento distinto.

Ejemplos dentro del Servicio "Trámite": Normal, Preferencial.
Ejemplos dentro del Servicio "Consulta Médica": Nueva Cita, Control.

**Sinónimos / Alias**
Subgrupo (legacy). En el sector salud: tipo de consulta, tipo de paciente.

**Lo que NO es**

- Pertenece a un Servicio.
- Un Ticket pertenece a una Categoría específica dentro de su Servicio.
- Su prefijo forma parte del código del Ticket junto con el prefijo del Servicio.

**Reglas de negocio**

- Un Servicio puede tener una o más Categorías activas.
- Si un Servicio tiene una sola Categoría activa, el kiosko puede omitir ese paso de selección para simplificar la experiencia del cliente. La categoria se asigna automáticamente.
- Cada Categoria tiene un prefijo único dentro de su Servicio que forma parte del código del Ticket.
- Los nombres de las Categorías sin configurables por cliente. Los más comunes son Normal y Preferencial.

**Contexto**

- Kiosko: el cliente la selecciona después de elegir el Servicio.
- Administración: donde se crea y configura dentro de cada Servicio.
- Reportes: permite filtrar atenciones por tipo de cliente.

---

### Puesto de Atención

_(nombre anterior: Rol Equipo)_

**Definición**
Configuración que define cómo opera un punto de trabajo dentro de la Sucursal. Determina el tipo de función que cumple (atender tickets, emitir tickets o mostrar llamados), qué Servicios gestiona y qué Usuarios o Dispositivos están asociados a él.
Es el elemento central de la configuración operativa del sistema: sin un Puesto de Atención correctamente configurado, ni los asesores, ni los kioskos, ni los monitores pueden operar.

Ejemplos: Ventanilla Preferencial, Monitor Plataforma, Kiosko Caja

**Sinónimos / Alias**
Rol Equipo (legacy). Algunos clientes lo llaman "perfil de trabajo" o "configuración de estación".

**Lo que NO es**

- No es una Ventanilla física. La Ventanilla es el módulo físico; el Puesto de Atención es la configuración lógica que define como se usa.
- No es un Usuario ni un Dispositivo, aunque ambos se asocian a él.
- No es un Rol de seguridad del sistema (no controla permisos de acceso al panel de administración).

**Tipos de Puesto de Atención:**

- **Ventanilla:** para asesores humanos que llaman y atienden tickets.
- **Kiosko:** para dispositivos que emiten tickets.
- **Monitor:** para dispositivos que muestran llamados.

**Relaciones**

- Pertenece a una Sucursal
- Tiene asociados uno o más Usuarios (asesores) sí es de tipo Ventanilla, o uno o más Dispositivos si es de tipo Kiosko o Monitor.
- Tiene configurados uno o más Servicios.
- Si es de tipo Ventanilla: cada Servicio tiene configuradas las proporciones por Categoría para el algoritmo de llamado.
- Si es de tipo Kiosko o Monitor: solo define qué Servicios mostrar, sin proporciones.

**Configuración de Servicios según tipo:**

Tipo Ventanilla:
Servicio (con peso entre servicios)
└── Categoría → proporción de llamado

Ejemplo:
INFORMACIÓN (peso: 2)
└── PREFERENCIAL: 100
└── NORMAL: 1

TRÁMITE (peso: 1)
└── PREFERENCIAL: 100
└── NORMAL: 1

Tipo Kiosko y Monitor:
Lista de Servicios a mostrar (sin pesos ni proporciones)

Ejemplo: INFORMACIÓN, TRÁMITE, VENTAS, RECLAMO

**Reglas de negocio**

- Un Puesto de Atención de tipo Ventanilla debe tener al menos un Servicio con al menos una Categoría y su proporcion configurada para poder operar.
- Un Puesto de Atención de tipo Kiosko o Monitor debe tener al menos un Servicio asignado para poder operar.
- Las proporciones por Categoria solo aplican a Puestos de tipo Ventanilla. Para Kiosko y Monitor no tienen efecto.
- Un Puesto de Atención tiene un tipo fijo que no puede cambiarse una vez creado.

**Contexto**

- Administración: donde se crea y configura completamente
- Ventanilla: el asesor opera bajo la configuración de su Puesto
- Kiosko: el dispositivo opera bajo la configuración de su Puesto
- Monitor: el dispositivo muestra los Servicios configurados en su Puesto

---

### Usuario

**Definición**
Persona humana registrada en el sistema que interactúa con él de forma activa durante su jornada laboral. A diferencia de un Dispositivo, un Usuario representa una persona con identidad, permisos y responsabilidades operativas dentro de la Sucursal.

Ejemplos: asesores de ventanilla, supervisores, administradores.

**Sinónimos / Alias**
Asesor (cuando el usuario es de tipo operativo). Agente (término usado en sistemas internacionales).

**Lo que NO es**

- No es un Dispositivo. Los kioskos y monitores son Dispositivos, no Usuarios.
- No es un Puesto de Atención. El Usuario se asocia a un Puesto pero no es el Puesto.

**Relaciones**

- Pertenece a una Sucursal y a un Área.
- Está asociado a un Puesto de Atención.
- Tiene una Ventanilla asignada por defecto si su Puesto es de tipo Ventanilla.
- Genera una Sesión de Trabajo al hacer login.

**Reglas de negocio**

- Un Usuario solo puede estar asociado a un Puesto de Atención a la vez.
- Un Usuario de tipo operativo (asesor) debe tener al menos una Ventanilla asignada en su Puesto para poder iniciar una Sesión de Trabajo.

**Contexto**

- Administración: donde se crea y configura
- Ventanilla: donde opera durante su jornada
- Reportes: unidad de análisis de productividad

---

### Ventanilla

**Definición**
Módulo físico de atención ubicado dentro de un Área de la Sucursal donde un asesor se sitúa para llamar y atender tickets. Es el punto de contacto presencial entre el asesor y el cliente durante la atención.

Ejemplos: Ventanilla 1, Ventanilla 2, Módulo 3.

**Sinónimos / Alias**
Módulo, caja, counter (término internacional).

**Lo que NO es**

- No es un Puesto de Atención. La Ventanilla es el espacio físico; el Puesto de Atención es la configuración lógica de cómo se trabaja en ese espacio.
- No es un Usuario. El asesor que trabaja en la ventanilla es el Usuario. El asesor que trabaja en la Ventanilla es el Usuario.
- No es un Dispositivo. La Ventanilla no hace login ni tiene configuración de software.

**Estados:**

LIBRE
Sin asesores asignados en configuración.
Cualquier asesor puede seleccionara al hacer login.

ASIGNADA / DESOCUPADA
Tiene asesores asignados en configuración.
Ninguno está actuvi en este momento.
Solo los asesores asignados a ella pueden confirmarla al hacer login.

OCUPADA
Tiene un asesor activo en este momento.
Ningún otro asesor puede entrar.

**Relaciones**

- Pertenece a un Área dentro de una Sucursal.
- Puede tener uno o más Usuarios (asesores) asignados en configuración.
- En un momento dado, solo un Usuario puede estar activo en ella (estado OCUPADA).

**Reglas de negocio**

- Una Ventanilla solo puede tener un asesor activo a la vez.
- Un asesor puede confirmar su Ventanilla asignada al hacer login solo si está desocupada.
- Si la Ventanilla asignada está ocupada, el asesor puede seleccionar cualquier Ventanilla LIBRE.
- Un asesor no puede seleccionar una Ventanilla ASIGNADA a otros asesores aunque esté desocupada.
- Para liberar una Ventanilla OCUPADA, el asesor activo debe cerrar su sesión.
- El administrador puede reasignar asesores a Ventanillas LIBRES o ASIGNADAS desocupadas, pero no a Ventanillas OCUPADAS.

**Contexto**

- Administración: donde se crea, se asignan asesores y se configura
- Ventanilla: donde el asesor opera durante su Sesión de Trabajo
- Monitor: muestra el número de Ventanilla al anunciar un ticket llamado
- Reportes: unidad de análisis de atenciones por módulo físico

---

### Sesión de Trabajo

**Definición**
Registro activo que vincula a un Usuario (asesor) con una Ventanilla física en un momento dado. Se crea automáticamente cuando el asesor hace login y confirma o selecciona su ventanilla. Se cierra cuando el asesor termina su jornada o cambia de ventanilla.

**Sinónimos / Alias**
Ninguno en el sistema legacy. Concepto nuevo en el sistema modernizado.

**Lo que NO es**

- No es una sesión de autenticación del sistema (login/logout). La Sesión de Trabajo es un concepto de negocio, no de Seguridad.
- No es una asignación permanente. La asignación permanente vive en la configuración del Puesto de Atención.

**Relaciones**

- Pertenece a un Usuario.
- Está vinculada a una Ventanilla.
- Está vinculada al Puesto de Atención del Usuario.

**Ciclo de vida**

ACTIVA
El asesor está trabajando en la ventanilla ahora mismo.
↓ Asesor cierra sesión o el admin la cierra
CERRADA
La ventanilla queda disponible (ASIGNADA/DESOCUPADA).

**Reglas de negocio**

- Solo puede existir un Sesión de Trabajo activa por Ventanilla en un momento dado.
- Solo puede existir un Sesión de Trabajo activa por Usuario en un momento dado.
- La Sesión de Trabajo se crea automáticamente al hacer login. No la crea el administrador manualmente.
- Genera un historial natural de en qué ventanilla trabajó cada asesor cada día, útil para reportes.

**Contexto**

- Ventanilla: se crea al iniciar la jornada del asesor
- Administración: el supervisor puede ver las sesiones activas en un tiempo real y cerrarlas si es necesario
- Reportes: fuente de datos para análisis de productividad por asesor y por ventanilla

---

### Ticket

**Definición**
Comprobante digital emitido por el sistema que representa el derecho de un cliente a ser atendido en un orden determinado dentro de una cola de atención. Es el elemento centrar del sistema: todo el flujo de atención gira alrededor de él desde que el cliente llega hasta que es atendido o se retira.

**Sinónimos / Alias**
Turno, número de atención, ficha.

**Lo que NO es**

- No es un ticket de soporte técnico (helpdesk).
- No es una reserva o cita previa: se emite en el momento en que el cliente llega físicamente.
- No es permanente: pertenece al día en que fue emitido. La numeración reinicia cada día.

**Origen**
Un Ticket es generado únicamente por el cliente desde el kiosko, seleccionando el Servicio y la Categoría.

**Código del Ticket**
Código alfanumérico visible para el cliente, compuesto por el prefijo del Servicio, el prefijo de la Categoría y un número secuencial reiniciado cada día.

Estructura: [prefijo Servicio][prefijo Categoria]-[número]
Ejemplo: IN-001 - A: prefijo del Servicio (ej. "Informes") - N: prefijo de la Categoría (ej. "Normal") - 001: número secuencial del día

**Relaciones**

- Pertenece a un Servicio.
- Pertenece a una Categoria dentro de ese Servicio.
- Es atendido por una Ventanilla.
- Puede ser Derivado a otro Servicio, conservando su código original. El sistema genera un nuevo registro interno vinculado al Ticket original.
- Forma parte de una Cola.

**Ciclo de vida**
GENERADO -> LLAMANDO -> EN_ATENCION -> FINALIZADO
↘ DERIVADO
↘ AUSENTE -> REACTIVADO -> EN_ATENCION
GENERADO -> ANULADO
GENERADO -> CANCELADO

**Reglas de negocio**

- Un ticket solo puede ser atendido por una Ventanilla asignada al Servicio al que pertenece.
- Un ticket en estado AUSENTE puede ser Reactivado dentro del mismo día, según configuración de la sucursal.
- La numeración del código reinicia cada día a las 00:00.
- Al Derivar un Ticket, el Ticket original pasa a estado DERIVADO y el sistema crea un nuevo registro interno en el Servicio destino. El código visible para el cliente se mantiene.
- Un cliente no puede tener más de un Ticket activo para el mismo Servicio al mismo tiempo. Si intenta emitir un segundo Ticket para un Servicio en el que ya tiene uno en estado GENERADO o LLAMANDO, el sistema lo rechaza e informa al cliente que ya tiene un turno activo para ese Servicio.

**Contexto**

- Kiosko: donde se genera
- Ventanilla: donde se llama y atiende
- Monitor: donde se muestra al público
- Consultor: donde se visualiza el estado de la cola
- Reportes: donde se analiza

---

## Procesos

<!-- términos aquí -->

## Estados

<!-- términos aquí -->

## Configuración

<!-- términos aquí -->

## Reglas de negocio

## Reglas de Negocio — ICLASSQ

### Ticket

RN-001: Un cliente no puede tener más de un Ticket
activo para el mismo Servicio simultáneamente.
Si intenta emitir un segundo Ticket para un
Servicio en el que ya tiene uno en estado
EN_ESPERA o LLAMADO, el sistema lo rechaza
e informa al cliente.

RN-002: Los Tickets se generan únicamente desde
el Kiosko por el cliente. Los asesores no
pueden generar Tickets desde la Ventanilla.

RN-003: Un Ticket solo puede ser atendido por una
Ventanilla cuyo Puesto de Atención tenga
asignado el Servicio al que pertenece el Ticket.

RN-004: Un Ticket en estado FINALIZADO no puede
cambiar de estado.

RN-005: Un Ticket en estado AUSENTE puede ser
Reactivado dentro del mismo día, según
configuración de la Sucursal.

RN-006: Al Derivar un Ticket, el Ticket original
pasa a estado DERIVADO y el sistema crea
un nuevo registro interno en el Servicio
destino. El código visible para el cliente
se mantiene igual.

RN-007: La numeración del código de Ticket reinicia
cada día a las 00:00.

### Puesto de Atención y Servicios

RN-008: Las proporciones por Categoría en la
configuración de llamado solo aplican a
Puestos de tipo Ventanilla. Los Puestos
de tipo Kiosko y Monitor únicamente definen
qué Servicios muestran, sin proporciones.

RN-009: Un Puesto de tipo Ventanilla debe tener
al menos un Servicio con al menos una
Categoría y su proporción configurada
para poder operar.

RN-010: Un Puesto de tipo Kiosko o Monitor debe
tener al menos un Servicio asignado
para poder operar.

RN-011: Un Servicio debe tener al menos una
Categoría activa para poder emitir Tickets.

RN-012: Si un Servicio tiene una sola Categoría
activa, el Kiosko puede ocultar ese paso
de selección. La Categoría se asigna
automáticamente al Ticket.

RN-013: Un Puesto de Atención tiene un tipo fijo
(Ventanilla, Kiosko, Monitor) que no puede
cambiarse una vez creado.

### Ventanilla y Sesión de Trabajo

RN-014: Una Ventanilla solo puede tener un asesor
activo a la vez.

RN-015: Al hacer login, el sistema sugiere al asesor
su Ventanilla asignada por defecto.
Si está desocupada, el asesor la confirma.
Si está ocupada, el asesor puede seleccionar
cualquier Ventanilla LIBRE.

RN-016: Un asesor no puede seleccionar al hacer
login una Ventanilla ASIGNADA a otros
asesores, aunque esté desocupada en
ese momento.

RN-017: Para liberar una Ventanilla OCUPADA,
el asesor activo debe cerrar su sesión.
Solo entonces queda disponible para
reasignación.

RN-018: El administrador puede reasignar asesores
a Ventanillas LIBRES o ASIGNADAS desocupadas.
No puede reasignar a Ventanillas OCUPADAS.

RN-019: Solo puede existir una Sesión de Trabajo
activa por Ventanilla en un momento dado.

RN-020: Solo puede existir una Sesión de Trabajo
activa por Usuario en un momento dado.

RN-021: La Sesión de Trabajo se crea automáticamente
cuando el asesor hace login y confirma
o selecciona su Ventanilla. No la crea
el administrador manualmente.

### Dispositivo

RN-022: Un Dispositivo tiene un tipo fijo
(Kiosko o Monitor) que no puede cambiarse
sin intervención del administrador.

RN-023: La IP del Dispositivo se registra
automáticamente al hacer login.
No se ingresa manualmente.

RN-024: Un Dispositivo está asociado a un único
Puesto de Atención.

### Servicio y Categoría

RN-025: Un Servicio tiene un prefijo único dentro
de la Sucursal que forma parte del código
del Ticket.

RN-026: Un Servicio puede estar activo o inactivo.
Si está inactivo no aparece en el Kiosko.

RN-027: Cada Categoría tiene un prefijo único
dentro de su Servicio que forma parte
del código del Ticket.

RN-028: Los nombres de las Categorías son
configurables por cliente. Los más
comunes son Normal y Preferencial.
