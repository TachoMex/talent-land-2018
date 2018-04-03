La importancia de la auditabilidad en los sistemas en producción

Gilberto Vargas

Kueski

---
¿Qué es la auditabilidad?

+ Habilidad para determinar las causas de un suceso
+ Conjunto de técnicas y buenas prácticas
---
Ejemplo

Para los ejemplos utilizaremos un caso de un sistema de
manufactura de "carritos".
---
Ejemplos

- Salió un carrito con 5 llantas (debería tener 4). ¿En que punto ocurrió el problema?
- Los carritos empezaron a salir todos de color azul, ¿quíen y cuándo se aprobó?
- ¿Quién puso el tornillo con las 3 líneas negras de la maquina que hace los volantes?
- ¿Quiénes tienen acceso a la zona de empaquetamiento y cuántas veces han entrado en la semana?
---
Sistema en producción

- Se refiere a las herramientas que se encargan de transformar la materia prima
en productos.
- En el caso de muchos sistemas de TI, la materia prima datos.
- Es la principal (si no es que la única) fuente de ingresos de una empesa.
- Tocar un sistema en producción sin autorización podría tener graves consecuencias legales.
---
Cosas a tomar en cuenta

- ¿Qué pasa si se apaga la máquina que hace la pintura para los carritos?
- ¿Qué pasa si alguien pone un material que debe ir en una máquina?
- ¿Puede un empleado ir a tomar cosas de la bodega y venderlas en el mercado negro?
- Las máquinas necesitan mantenimiento.
- ¿Qué pasa si hay un error al momento de instalar una máquina?
- ¿Y al dar mantenimiento?
---
¿Por qué es importante la auditabilidad?

- Van a pasar cosas... feas
  - Incidencias.
  - Errores humanos
  - Ataques
- Deslindar de culpas a los empleados
---
Ventajas de la auditabilidad

- Reproducir los eventos de producción en un ambiente de pruebas
- Depurar errores en las aplicaciones
---
Respondiendo las preguntas

- Salió un carrito con 5 llantas. ¿En que punto ocurrió el problema?
Se revisó el video de las máquinas y se encontró que bajo cierto ángulo,
el robot que toma las llantas recoge 2 en lugar de una.
---
Los carritos empezaron a salir todos de color azul, ¿quíen y cuándo se aprobó?

El ticket [PD-222] solicita que por el día del oceano, toda la producción sea de color azul.
Fue aprobado por un manager, lo realizaron 2 ingenieros y fue revisado por otros 2.
---
¿Quién puso el tornillo con las 3 líneas negras de la maquina que hace los volantes?

Lo hizo un becario, el 25 de Julio del 2016 a las 4:59 de la tarde. Lo hicimos revisando
las camaras de seguridad.
---
¿Quiénes tienen acceso a la zona de empaquetamiento y cuántas veces han entrado en la semana?

En total son 6 empleados. Existe un documento que dice cuantas veces han entrado cada uno debido a que hay un lector de huella en la entrada.
---
Los carritos ahora son préstamos

Los préstamos en kueski también son una línea de ensamble.
Se deben realizar varios pasos para armar un préstamo y que este pueda ser entregado al cliente.
---
Herramientas de auditabilidad en el software

- Logs de las aplicaciones
- Logs al momento de tocar los servidores
- Control de tickets
- Control de versiones
---
¿Qué son los logs?

Archivos gigantes que registran mensajes sobre lo que va pasando en las aplicaciones.
---
¿Cómo escribir logs?
---
Objetivos de un mensaje

- Explicar que está pasando
- Dar detalles que ayuden a determinar que operaciones se ejecutaron
- Reconstuir los hechos
---
Problemas cuando trabajas con un log

- Aplicaciones multihilos o multiservidor
- Integrar con herramientas para búsqueda de logs
- El balance entre ser muy verboso y perder información
- Búsqueda
---
Componentes de un mensaje

- Severidad (DEBUG, INFO, WARN, ERROR, FATAL)
- Fecha
- Identificador del host, proceso, hilo, etc.
- Detalles de la aplicación
---
Soluciones

Aplicaciones multihilos o multiservidor.

Al momento de recolectar los logs se guarda la IP del host.
Los mensajes contienen el identificador de proceso y del hilo,

---
- Integrar con herramientas para búsqueda de logs

Existen herramientas que están escuchando a los archivos. Si escribes todo a un
archivo es fácil de integrar.
---
- El balance entre ser muy verboso y perder información

Ante errores muestra todos los detalles que necesitas para reconstruir el hecho.
Si la consulta fue satisfactoria, muestra los identificadores de los objetos modificados.
---
- Búsqueda

La busqueda de cadenas en los logs se la dejamos a las herramientas de administración de logs.
Utiliza identificadores a nivel usuario/proceso de modo que puedas rastrear todos los registros
para una misma entidad.
---
Logs de Kueski (cute-logger)

Una línea, formato CSV.

Fecha, Severidad, proceso, hilo, modulo, detalles.
```
2018-03-21 14:43:39 -0600,INFO,133,2aecdedf2f50,AuthServerClient,["Requested performed correctly",{"path":"http://localhost:4321/api/users"}]
```
---
Logs para los humanos

Parseo de los logs y motrado de una forma mas "visual"
```
2018-03-21 14:43:39 -0600 INFO 307-2aecdedf2f50 (AuthServerClient)
[
    [0] "Requested performed correctly",
    [1] {
        "path" => "http://localhost:4321/api/users"
    }
]
```
---
Control de versiones

Son programas que ayudan a administrar el código fuente.
---
- Ante una incidencia, ¿A quién debería pedir ayuda?
- ¿Cómo saber que versión del software estuvo en producción en cada día?
- Regresar la aplicación a una fecha determinada

---
- Ante una incidencia, ¿A quién debería pedir ayuda?

Git tiene la capacidad de decir quien escribió cada línea de código
y la fecha. La historia de cada cambio debería conener información
sobre la tarea que necesitaba el cambio.

Los pull request almacenan que usuarios aprobaron el merge.

Cada commit tiene una refencia a la tarea que lo requirió.
---
- ¿Cómo saber que versión del software estuvo en producción en cada día?

Versionamiento de "paquetes" al momento de publicar una nueva versión.

Docker permite guardar imagenes del estado de un componente.

Etiquetas en git
---
- Regresar la aplicación a una fecha determinada

Teniendo las etiquetas o las imagenes, puedes regresar la aplicación al estado anterior

---
Liberación de nuevas versiones de las aplicaciones

- ¿Cómo volvemos a como estabamos antes de empezar si algo falla?
- ¿Cómo podemos determinar si un problema fue causado por un error en un proceso de cambio?
---
¿Cómo volvemos a como estabamos antes de empezar si algo falla?

- Respaldos
- Nunca realizar una operación que no puede ser revertida
---
¿Cómo podemos determinar si un problema fue causado por un error en un proceso de cambio?

- Registrar todas los comandos que se introdujeron al sistema con sus salidas
- Logs con los parametros de entrada
---
Incidencias

- Producidas por errores humanos
- Producidas por un ataque
- Capacidad para determinar el impacto total del evento
- Ante una incidencia que se había tenido previamente, ¿Cómo podemos determinar que se hizo en aquella ocasión?
---
Producidas por errores humanos

- Validación de las entradas en las herramientas
- Documentación de todos los errores pasados
- Siempre se buscan soluciones y no culpables
---
Producidas por un ataque

- Robustecimiento de la aplicación
- Mejorar el acceso a la plataforma
- Pentesting
---
Capacidad de determinar el impacto total del evento

- Depende mucho de los logs
- Ayudan a determinar la causa del evento
---
Ante una incidencia que se había tenido previamente, ¿Cómo podemos determinar que se hizo en aquella ocasión?

- Logs, logs, logs.
- Registrar todas los comandos que se introdujeron al sistema con sus salidas
---
Conclusión

- Logs
- Logs
- Es mas complicado mantener un sistema en producción que desarrollarlo.
- Logs
- Siempre hay que dejar evidencia de absolutamente todo lo que tenga sentido.
- ¿Ya dije logs?
---
Contacto

¡Gracias por su atención!

http://github.com/tachomex/
https://www.linkedin.com/in/tachomex/
