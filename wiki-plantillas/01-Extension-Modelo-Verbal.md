# Paso 1 — Extensión del modelo verbal

## Ambigüedades identificadas en el modelo verbal

Al leer el enunciado el equipo encontró puntos que no quedan definidos y que resolverá en el Paso 2:

- **Inversor y usuario final** aparecen como actores distintos en el contexto, pero la descripción funcional usa "usuarios" para ambos. Se asume que el inversor es quien compra y el usuario final quien opera la instalación, y que pueden ser la misma persona.
- **Entidades financieras**: se nombran como actores, pero no se dice qué hacen dentro de la plataforma (publicar opciones de crédito, aprobar solicitudes o solo ser referidas).
- **Asesoría personalizada**: no se dice quién la presta (un asesor humano, un algoritmo de recomendación o ambos).
- **"Tiempo real"**: no se define de dónde salen los datos de rendimiento (inversores, medidores, carga manual) ni cada cuánto llegan.
- **Qué pasa cuando una instalación deja de rendir**: el seguimiento muestra gráficos, pero no se contempla ninguna acción. De esta ambigüedad nace la extensión.

## Funcionalidad adicional propuesta por el equipo

El enunciado describe un sistema de seguimiento que muestra, en tiempo real, el rendimiento de cada instalación mediante gráficos y estadísticas en el panel del usuario, y un panel para instaladores donde reciben órdenes y actualizan el estado de las instalaciones. Sin embargo, no dice qué pasa cuando una instalación deja de rendir: un panel sucio, sombreado, un inversor apagado o un cable suelto reducen la generación durante semanas sin que el usuario lo note, porque mirar el gráfico todos los días no es algo que la mayoría haga.

El equipo propone agregar un **servicio de alertas de bajo rendimiento con solicitud de mantenimiento al instalador**. La plataforma compara la energía generada por la instalación con la que debería estar generando en las mismas condiciones. Como referencia usa, en este orden según disponibilidad, la irradiancia medida o estimada para la zona, el promedio de instalaciones cercanas de capacidad similar y, para instalaciones con más de un mes de datos, su propio histórico a esa hora. Cuando la generación queda más de un 20 % por debajo de esa referencia durante tres horas seguidas de sol, la plataforma avisa al usuario en su panel y por notificación, y le permite, desde la misma alerta, enviar una orden de mantenimiento al instalador que hizo la instalación. El instalador la recibe en su panel, la atiende y actualiza su estado, de modo que el usuario ve cuándo quedó resuelta.

Esta extensión conecta tres piezas que el enunciado ya tiene (seguimiento en tiempo real, panel del instalador y notificaciones de órdenes) y responde a una característica propia del dominio: la inversión en un sistema solar se recupera con la energía que genera, así que cada día de bajo rendimiento es dinero perdido para el inversor.

## Requisito de calidad asociado

**Oportunidad de la alerta.** Una vez se cumple la condición de bajo rendimiento (más de 20 % por debajo de la referencia durante tres horas seguidas de sol), la plataforma debe crear la alerta y notificar al usuario en **15 minutos o menos** desde que recibe la lectura que completa esa condición. Se medirá como el tiempo entre esa lectura y la notificación, y se aceptará si se cumple en al menos el 95 % de las alertas de un mes.

Una interrupción de datos (sin lecturas) no cuenta como bajo rendimiento: produce un aviso distinto de "sin datos recientes", para no enviar órdenes de mantenimiento falsas.

## Justificación de la decisión

- **Es coherente con el dominio.** La degradación y las fallas de paneles e inversores son el problema operativo más común en instalaciones solares residenciales y de pymes, y es justo el tipo de riesgo no obvio que el enunciado deja fuera.
- **Reutiliza lo que ya pide el enunciado.** No agrega actores: usa los datos del seguimiento en tiempo real y el panel de instaladores con sus órdenes. Agrega una sola integración externa, la fuente de irradiancia para calcular la generación esperada, documentada como requisito de interfaz en el Paso 2.
- **Refuerza la meta de negocio.** El enunciado exige una satisfacción del cliente superior al 80 %; un usuario que se entera a tiempo de una falla y la resuelve desde la plataforma tiene motivos para quedarse en ella.
- **Es defendible en la sustentación.** Da pie a riesgos concretos (integración con inversores de distintos fabricantes, calidad de las lecturas) y a un requisito de calidad medible.

---
