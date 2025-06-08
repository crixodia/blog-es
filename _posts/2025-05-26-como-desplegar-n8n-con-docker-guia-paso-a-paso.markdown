---
title: "Cómo desplegar n8n con Docker: Guía paso a paso"
categories: [DevOps, Docker]
tags: [n8n, Docker, Automatización, Autoalojamiento]
date: 2025-05-26 20:00:00 -0500
math: false
mermaid: false
pin: false
media_subpath: /assets/img/posts/como-desplegar-n8n-con-docker-guia-paso-a-paso
image: como-desplegar-n8n-con-docker-guia-paso-a-paso.webp
---

Los modelos de lenguaje grandes han reavivado el interés en las plataformas de automatización. Una de ellas es [n8n](https://n8n.io/), que brinda a los equipos técnicos la flexibilidad del no-code. Esta herramienta cuenta con más de 400 integraciones y capacidades nativas de IA, permitiéndonos interactuar con LLMs usando APIs o incluso nuestros propios modelos autoalojados con [Ollama](https://ollama.com/).

En este artículo, vamos a desplegar la versión comunitaria de n8n en nuestro propio servidor. Para lograrlo, asegúrate de tener instalado previamente [Docker](https://www.docker.com/) y [Docker Compose](https://docs.docker.com/compose/). Cabe destacar que esta guía está pensada para usuarios de Linux; sin embargo, también funciona en Windows a través de [Docker Desktop](https://www.docker.com/products/docker-desktop/).

## Archivo Docker Compose

Vamos a obtener la imagen oficial desde [Docker Hub](https://hub.docker.com/). Como especifica la documentación, hay algunas variables de entorno que debemos revisar:

* **`WEBHOOK_URL`**: La URL que n8n usará para generar URLs de webhooks. Esto es importante para que los webhooks funcionen correctamente, especialmente si ejecutas n8n detrás de un proxy inverso o en un dominio diferente.
* **`N8N_HOST`**: La dirección IP o el nombre de dominio del servidor donde se ejecuta n8n.
* **`VUE_APP_URL_BASE_API`**: La URL base para la API de n8n. Es utilizada por el frontend para hacer llamadas a la API. Debe coincidir con `WEBHOOK_URL` y `N8N_HOST`.
* **`N8N_PROTOCOL`**: El protocolo que utiliza n8n, ya sea `http` o `https`.
* **`GENERIC_TIMEZONE`**: Especifica la zona horaria que debe usar n8n. Afecta la programación de tareas.
* **`TZ`**: La zona horaria del sistema. Controla la salida de ciertos scripts y comandos.

Ahora que conocemos las variables de entorno, podemos crear un archivo `docker-compose.yml`. Este archivo define el servicio de n8n y su configuración. A continuación, un ejemplo de cómo configurarlo:

```yml
services:
  n8n:
    container_name: n8n
    image: n8nio/n8n:latest
    restart: always
    environment:
      GENERIC_TIMEZONE: America/Guayaquil
      TZ: America/Guayaquil
    ports:
      - 5678:5678
    volumes:
      - ./n8n:/home/node/.n8n
```

### Proxy inverso

Si utilizas un proxy inverso, deberás establecer las variables de entorno `WEBHOOK_URL`, `N8N_HOST`, `VUE_APP_URL_BASE_API` y `N8N_PROTOCOL` según corresponda. Aquí tienes un ejemplo de cómo modificar el archivo `docker-compose.yml` para una configuración con proxy inverso:

```yml
services:
  n8n:
    container_name: n8n
    image: n8nio/n8n:latest
    restart: always
    environment:
      WEBHOOK_URL: https://<tu-dominio>/
      N8N_HOST: <tu-dominio>
      VUE_APP_URL_BASE_API: https://<tu-dominio>/
      N8N_PROTOCOL: https
      GENERIC_TIMEZONE: America/Guayaquil
      TZ: America/Guayaquil
    ports:
      - 5678:5678
    volumes:
      - ./n8n:/home/node/.n8n
```

## Despliegue

Ahora que tenemos listo nuestro archivo `docker-compose.yml`, podemos desplegar n8n. Abre una terminal y navega hasta el directorio donde guardaste el archivo `docker-compose.yml`. Luego, ejecuta el siguiente comando:

```bash
docker-compose up -d
```

Este comando iniciará el servicio de n8n en modo desacoplado. Puedes verificar el estado del servicio ejecutando:

```bash
docker-compose ps
```

> Es posible que tu servidor requiera que uses `sudo` antes de ejecutar comandos de Docker.

Si todo está funcionando correctamente, deberías ver el servicio de n8n listado como "Up".

## Acceder a n8n

Puedes acceder a n8n abriendo tu navegador y navegando a `http://<la-ip-de-tu-servidor>:5678` o `https://<tu-dominio>` si usas un proxy inverso. Deberías ver la interfaz de n8n, donde puedes comenzar a crear flujos de trabajo y automatizaciones.

## Conclusión

En este artículo, cubrimos cómo desplegar n8n usando Docker Compose. También discutimos las variables de entorno necesarias y cómo configurar n8n para su uso con un proxy inverso. Con n8n, puedes automatizar tareas e integrar diversos servicios sin escribir código, lo que lo convierte en una herramienta poderosa para equipos técnicos.

Si tienes preguntas o necesitas más ayuda, no dudes en dejar un comentario abajo.

## Recursos adicionales

* [Documentación de n8n](https://docs.n8n.io/)
* [Documentación de Docker](https://docs.docker.com/)
* [Documentación de Docker Compose](https://docs.docker.com/compose/)
* [Documentación de Ollama](https://ollama.com/docs)
