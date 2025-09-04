# Jenkins pruebas
El propósito de este repositorio es hacer uso de Jenkins para poder hacer ci/cd del presente repositorio.

# Tabla de contenidos
- [Jenkins pruebas](#jenkins-pruebas)
- [Tabla de Contenidos](#tabla-de-contenidos)
    - [Docker Compose](#docker-compose)
    - [Jenkins](#jenkins)

# Docker Compose

# Jenkins
## Primeros pasos
Al arrancar por primera vez el docker-compose, deberá crear un volumen con el nombre especificado siguiendo este comando:

```bash
docker volume create jenkins-volume
```
Esto debido a que se especifica en el docker-compose que este volumen es externo, osea lo entrega el desarrollador.

Luego una vez levantado, acceda a ip-local:8080 (URL de Jenkins) y escriba la clave que aparecerá en los logs del contenedor de Jenkins.

Así, podrá crear su usuario y poder entrar a Jenkins.

## Pasos siguientes
Una vez completados los primeros pasos, dirijase a la configuración de Jenkins y seleccione Plugins, allí deberá instalar Generic Webhook Trigger, esto nos permitirá recibir hooks desde github configurandolo de manera sencilla.

Cree un nuevo pipeline y al configurarlo podrá observar que existe un apartado de Generic Webhook Trigger que deberá marcar para poder usar. Por otro lado, en github, en su repositorio especificamente deberá escribir en WebHooks la URL donde querrá enviar estos Hooks, 