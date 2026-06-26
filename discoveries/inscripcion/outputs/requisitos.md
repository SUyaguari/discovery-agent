# Requisitos Candidatos — Discovery: Inscripción Oratorio Vacacional

## Funcionales

- **[R-01]** El sistema debe presentar un único formulario de inscripción que clasifique automáticamente al niño en el grupo correspondiente según su fecha de nacimiento o edad ingresada.
  - Tipo: funcional
  - Origen: `receptor-inscripciones.md` · Receptor de Inscripciones / `coordinador-oratorio.md` · Coordinador del Oratorio / `representante-familia.md` · Representante de familia

- **[R-02]** El formulario debe registrar primero los datos del representante legal y luego permitir agregar uno o más niños vinculados a ese representante en la misma inscripción.
  - Tipo: funcional
  - Origen: `receptor-inscripciones.md` · Receptor de Inscripciones / `representante-familia.md` · Representante de familia

- **[R-03]** Los rangos de edad y la definición de grupos del Oratorio deben ser configurables por el equipo administrativo.
  - Tipo: funcional
  - Origen: `coordinador-oratorio.md` · Coordinador del Oratorio

- **[R-04]** El sistema debe permitir continuar con la inscripción aunque un documento (cédula) esté pendiente, marcando el pendiente para seguimiento posterior sin bloquear el proceso.
  - Tipo: funcional
  - Origen: `receptor-inscripciones.md` · Receptor de Inscripciones / `representante-familia.md` · Representante de familia

- **[R-05]** El sistema debe gestionar al menos cuatro estados de pago: pendiente, pendiente de verificación, verificado y rechazado/con observación.
  - Tipo: funcional
  - Origen: `tesorera-oratorio.md` · Tesorera del Oratorio / `representante-familia.md` · Representante de familia

- **[R-06]** Al confirmar el pago de una inscripción, el sistema debe habilitar automáticamente el envío del enlace de WhatsApp del grupo correspondiente al representante.
  - Tipo: funcional
  - Origen: `coordinador-oratorio.md` · Coordinador del Oratorio / `tesorera-oratorio.md` · Tesorera del Oratorio / `representante-familia.md` · Representante de familia

- **[R-07]** Cuando una familia tiene varios niños en grupos distintos, el mensaje de WhatsApp debe identificar claramente qué niño pertenece a qué grupo.
  - Tipo: funcional
  - Origen: `coordinador-oratorio.md` · Coordinador del Oratorio / `representante-familia.md` · Representante de familia

- **[R-08]** El sistema debe generar y enviar un comprobante de inscripción por WhatsApp al representante, incluyendo fecha, datos de los niños inscritos, estado del pago y un código o número de inscripción.
  - Tipo: funcional
  - Origen: `tesorera-oratorio.md` · Tesorera del Oratorio / `representante-familia.md` · Representante de familia

- **[R-09]** El sistema debe presentar un panel de seguimiento con niños por grupo, estado de pago, estado de documentos, datos del representante, número de WhatsApp y si ya se envió el enlace.
  - Tipo: funcional
  - Origen: `coordinador-oratorio.md` · Coordinador del Oratorio / `receptor-inscripciones.md` · Receptor de Inscripciones

- **[R-10]** El sistema debe permitir buscar inscripciones por nombre del niño o por cédula del representante.
  - Tipo: funcional
  - Origen: `receptor-inscripciones.md` · Receptor de Inscripciones

- **[R-11]** Para pagos en efectivo, el sistema debe registrar fecha, monto, la familia inscrita y el responsable que recibió el dinero.
  - Tipo: funcional
  - Origen: `tesorera-oratorio.md` · Tesorera del Oratorio

- **[R-12]** Para pagos por transferencia, el sistema debe mostrar representante, niños inscritos, monto, fecha y comprobante adjunto, y registrar quién realizó la verificación.
  - Tipo: funcional
  - Origen: `tesorera-oratorio.md` · Tesorera del Oratorio

- **[R-13]** El formulario de inscripción debe permitir registrar una observación de salud (alergias u otras condiciones médicas) por cada niño.
  - Tipo: funcional
  - Origen: `representante-familia.md` · Representante de familia

- **[R-14]** El representante debe poder visualizar el estado actual de su inscripción y del pago (pendiente / pendiente de verificación / verificado) sin necesidad de consultar al equipo por WhatsApp.
  - Tipo: funcional
  - Origen: `representante-familia.md` · Representante de familia

## No funcionales

- **[R-15]** El sistema debe ser accesible desde dispositivos móviles para permitir la inscripción, la carga de documentos fotográficos y la consulta del estado desde el celular.
  - Tipo: no funcional
  - Origen: `receptor-inscripciones.md` · Receptor de Inscripciones / `representante-familia.md` · Representante de familia

- **[R-16]** El tiempo de registro de una familia (representante + un niño + pago) no debe superar el tiempo del proceso manual actual en condiciones normales de uso.
  - Tipo: no funcional
  - Origen: `receptor-inscripciones.md` · Receptor de Inscripciones / `representante-familia.md` · Representante de familia
