# Jenkins guide
El propósito de este repositorio es guiar el uso de Jenkins para poder hacer ci/cd.

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

Luego una vez levantado, acceda a ip-local:8080 (URL de Jenkins que su web server debe conocer, en este caso fue configurado por Nginx y se accede a un dominio del tipo: example.com/ -> jenkins) y escriba la clave que aparecerá en los logs del contenedor de Jenkins.

Así, podrá crear su usuario y poder entrar a Jenkins.

## Pasos siguientes
Una vez completados los primeros pasos, dirijase a la configuración de Jenkins y seleccione Plugins, allí deberá instalar Generic Webhook Trigger, esto nos permitirá recibir hooks desde github configurandolo de manera sencilla.

Cree un nuevo pipeline y al configurarlo podrá observar que existe un apartado de Generic Webhook Trigger que deberá marcar para poder usar. Por otro lado, en github, en su repositorio especificamente deberá escribir en WebHooks la URL donde querrá enviar estos Hooks, que tiene la siguiente estructura en Jenkins: 

 http://JENKINS_URL/generic-webhook-trigger/invoke

 Si quisiera agregar un Token para mayor seguridad, bastaría con agregar /invoke?token=TOKEN_HERE a la URL.

 Dentro del payload que Github envía a través de este Hook se encuentra:

 {
  "ref": "refs/heads/development",
  "repository": {
    "name": "jenkins",
  }
  "ssh_url": "git@github.com:CesarRodriguezPardo/jenkins.git",
  "clone_url": "https://github.com/CesarRodriguezPardo/jenkins.git",
  "pusher": {
    "name": "CesarRodriguezPardo",
    "email": "115362848+CesarRodriguezPardo@users.noreply.github.com"
  },
}

eeeeee