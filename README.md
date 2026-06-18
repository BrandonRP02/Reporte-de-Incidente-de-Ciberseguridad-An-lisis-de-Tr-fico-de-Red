# Reporte de Incidente de Ciberseguridad: Análisis de Tráfico de Red

## Parte 1: Resumen del problema encontrado en el registro tcpdump
Como parte del protocolo DNS, se utilizó el protocolo UDP para contactar al servidor DNS y recuperar la dirección IP del nombre de dominio `yummyrecipesforme.com`. El protocolo ICMP se utilizó para responder con un mensaje de error, indicando problemas al contactar al servidor DNS. 

El mensaje UDP que va desde el navegador al servidor DNS se muestra en las dos primeras líneas de cada evento del registro. La respuesta de error ICMP del servidor DNS al navegador se muestra en la tercera y cuarta líneas de cada evento del registro con el mensaje de error: "udp port 53 unreachable". 

Dado que el puerto 53 está asociado con el tráfico del protocolo DNS, se evidencia que este es un problema con el servidor DNS. Los problemas con la ejecución del protocolo DNS son aún más claros porque el signo más (`+`) después del número de identificación de consulta `35084` indica banderas con el mensaje UDP, y el símbolo `A?` indica banderas con la ejecución de operaciones del protocolo DNS. Debido al mensaje de respuesta de error ICMP sobre el puerto 53, es altamente probable que el servidor DNS no esté respondiendo. Esta suposición está respaldada además por las banderas asociadas con el mensaje UDP saliente y la recuperación del nombre de dominio.

## Parte 2: Análisis de los datos y causas del incidente
El incidente ocurrió hoy a la 1:24 p.m. Los clientes notificaron a la organización que recibieron el mensaje "puerto de destino inalcanzable" cuando intentaron visitar el sitio web `yummyrecipesforme.com`. El equipo de ciberseguridad que proporciona servicios de TI a la organización cliente está investigando actualmente el problema para que los clientes puedan acceder al sitio web nuevamente. 

En la investigación del problema, se realizaron pruebas de rastreo de paquetes utilizando `tcpdump`. En el archivo de registro resultante, se descubrió que el puerto DNS 53 era inalcanzable. El siguiente paso es identificar si el servidor DNS está inactivo o si el tráfico al puerto 53 está bloqueado por el firewall. El servidor DNS podría estar inactivo debido a un ataque de Denegación de Servicio (DoS) exitoso o a una configuración incorrecta.
