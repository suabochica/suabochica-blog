+++
title = "Comunicación Cliente Servidor"
date = 2018-12-12T02:13:50Z
author = "Sergio L. Benítez D."
description = "Introducción a la serie de publicaciones para desarrollar la relación cliente-servidor de la web"
tags = [
    "HTTP",
]
+++

Esta serie va a tratar un tema **web**. _La comunicación cliente-servidor_. La web es solo una pequeña parte del Internet. Sin embargo es la única parte de Internet que la gente usa sin darse cuenta de que están usando la web. La web es una plataforma en donde los desarrolladores web publican sus ideas para el mundo.

En la web a un lenguaje común entre los servidores y los clientes. Cada vez que usted abre un navegador, cada vez que usted descarga una aplicación y cada vez que recibe un mensaje de _WhatsApp_  usted esta usando la web. O quizás, de una forma más genérica un cliente esta comunicando con un servidor y vice versa.

Pero, ¿Qué significa esto? ¿Qué sucede cuando se navega a un sitio web? ¿Como hace el teléfono para saber que alguien me envió un mensaje? ¿Qué capacidades tiene la web? y aún más importante ¿Cuáles son las limitaciones de esta comunicación?

En esta serie vamos a tratar de entender qué es la web, cómo podemos usarla para nuestro beneficio y así evitar errores que harán que su experiencia de usuario y su seguridad se vean afectadas. Para lograr el objetivo es necesario revisar las partes menos conocidas de **HTTP (Hypertext Transfer Protocol)**.

A continuación comparto la tabla de contenidos con los temas que se desarrollarán en las publicaciones de la serie:

## El ciclo solicitud/respuesta de HTTP

1. Solicitudes HTTP
2. Obteniendo una sola solicitud
3. Obteniendo múltiple solicitudes
4. Enviando datos con una solicitud POST
5. De `XHR` a `Fetch`

## HTTP/1

1.  El comando `netcat`
2.  Verbos HTTP
3.  Encabezados comunes de respuesta
4.  REST
5.  Fundamentos básicos de rendimiento
6.  Detalles de rendimiento

## HTTPS

1. Asegurando HTTP
3. TLS y autoridades certificadoras
4. TLS: Cartilla de criptografía
5. TLS: Hashing
6. Firmas de la autoridades de certificación
7. El apretón de manos de TLS
8. Contenidos mezclados

## HTTP/2

1. Problema de HTTP/1: Head of Line Blocking
2. Problema de HTTP/1: Encabezado sin comprimir
3. Problema de HTTP/1: Seguridad
4. Mejoras de HTTP/2
5. Trabajando con HTTP/2

## Seguridad

1. Orígenes
2. Anulando Same Origin Policy
3. CORS
4. Falló de seguridad: CSRF
5. Falló de seguridad: XSS
