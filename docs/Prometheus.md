[//]: # ()
[//]: # (# Prometheus)

[//]: # ()
[//]: # (Se utiliza [Actuator]&#40;https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html&#41; para exponer las métricas.)

[//]: # (Y [Micrometer]&#40;https://micrometer.io/docs&#41; para generarlas.)

[//]: # ()
[//]: # (Para poder utilizar estas métricas que generamos, es necesario tener Prometheus y Grafana. Esto será responsabilidad de cada equipo/gerencia.)

[//]: # ()
[//]: # (Además, recomendamos registrar en el post install de la aplicación el registro de la app en Prometheus, para que venga a consumir nuestras métricas generadas.)

[//]: # ()
[//]: # ()
[//]: # (### Actuator)

[//]: # ()
[//]: # (Consultando *"/actuator"* se puede ver lo que tenemos activado.)

[//]: # ()
[//]: # (- Métricas que expone la app: *"/actuator/metrics"*)

[//]: # ()
[//]: # (- Detalle de una métrica particular: *"/actuator/metrics/{metricName}"*)

[//]: # ()
[//]: # (- Métricas con el formato para ser consumidas por prometheus: *"/actuator/prometheus"*)

[//]: # ()
[//]: # (Como se puede ver en la documentación,)

[//]: # ([Actuator]&#40;https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html&#41; nos permite activar más cosas configurables mediante properties,)

[//]: # (en nuestro caso decidimos la siguiente configuración básica, pero recomendamos analizar en cada caso:)

[//]: # (```)

[//]: # (management.endpoints.web.exposure.include=metrics,prometheus)

[//]: # (management.endpoint.metrics.enabled=true)

[//]: # (management.endpoint.prometheus.enabled=true)

[//]: # (management.metrics.export.prometheus.enabled=true)

[//]: # (```)

[//]: # (Con esta configuración podremos tener métricas de la JVM y de Jetty, además de las que generemos nosotros, como veremos más adelante.)

[//]: # ()
[//]: # ()
[//]: # (### Counters)

[//]: # (Mediante el service [PrometheusService]&#40;https://github.com/despegar/java-template/blob/main/src/main/java/com/despegar/javatemplate/service/PrometheusService.java&#41;,)

[//]: # (podremos gestionar contadores propios, con tags particulares. También se podrían implementar, de ser necesario, otro tipo de métricas, como por ejemplo gauges.)

[//]: # ()
[//]: # ()
[//]: # (### @Timed)

[//]: # (Esta annotation permite medir el tiempo de ejecución de un método y calcular los percentiles que necesitemos,)

[//]: # (para luego crear nuestros dashboards. Recomendamos utilizarlo en los puntos de entrada &#40;Controllers&#41; y salida &#40;Clients, Repositories&#41; de la app.)

[//]: # (Se utiliza de la siguiente manera:)

[//]: # (```)

[//]: # (@Timed&#40;percentiles = { 0.5, 0.8, 0.95, 0.99, 0.999 }, histogram = true, value = "name"&#41;)

[//]: # (```)

[//]: # ()
[//]: # (### Dashboards)

[//]: # (Podemos crear nuestros propios dashboards, pero además Grafana nos provee templates que pueden ser de gran ayuda, como [por ejemplo]&#40;https://grafana.com/grafana/dashboards/12464&#41;. )

[//]: # ()
[//]: # (Recomendamos como buena práctica, almacenar un export de nuestros dashboards en el repositorio de la app, para tener a modo de backup.)