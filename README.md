# Jenkins guide
El propósito de este repositorio es guiar el uso de Jenkins para poder hacer ci/cd.

# Tabla de contenidos
- [Jenkins pruebas](#jenkins-pruebas)
- [Tabla de Contenidos](#tabla-de-contenidos)
    - [Inicio](#Inicio)
          - [Pasos previos](#pasos-previos)
            - [Webhook Generic Trigger](#webhook-generic-trigger)
            - [SSH Agent](#ssh-agent)

# Inicio
## Ejecución
Al arrancar por primera vez el docker-compose, deberá crear un volumen con el nombre especificado siguiendo este comando:

```bash
docker volume create jenkins-volume
```

Esto debido a que se especifica en el docker-compose que este volumen es externo, osea lo entrega el desarrollador.

Luego una vez levantado, acceda a ip-local:8080 (URL de Jenkins que su web server debe conocer, en este caso fue configurado por Nginx y se accede a un dominio del tipo: example.com/ -> jenkins) y escriba la clave que aparecerá en los logs del contenedor de Jenkins.

Así, podrá crear su usuario y poder entrar a Jenkins.

## Pasos siguientes
Instalar Webhook Generic Trigger y SSH Agent en Jenkins. (Además de todos los plugins recomendados)

### Webhook Generic Trigger
Este plugin nos permite recibir hooks desde GitHub configurandolo de manera sencilla.

Nos provee una URL donde está hosteado Jenkins donde Github podrá enviar los webhooks, esta tiene la siguiente estructura:

```bash
 http://JENKINS_URL/generic-webhook-trigger/invoke
```
Si se quisiera agregar un Token para mayor seguridad, bastaría con agregar /invoke?token=TOKEN_HERE a la URL, el token se configura en la misma pestaña del plugin.

Dentro del payload que Github envía a través de este Hook se encuentra:

```bash
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
```

Esto nos permitirá trabajar con esta útil información, podemos crear parámetros con la siguiente estructura:

```bash
nombre del parámetro: repositoryName
expresión: $.repository.name (esto dice: del payload accede a ->repository->name)
```
Debe seleccionar JSONPath.

### SSH Agent
Este plugin nos permite poder conectarnos a una VM a través de un sshagent con credenciales específicas y continue los pasos:
  1) Cree sus llaves públicas/privadas
  2) En Jenkins -> Manage Jenkins -> Credentials, una vez allí, debe crear una nueva credencial SSH, lo más imporante es poder asignarle un ID para poder acceder a el rápidamente, un username que será por el que nos conectaremos vía SSH y por último, una llave privada generada anteriormente.

## Configuración del pipeline
Una vez realizados todos los pasos anteriores puede proceder a definir el pipeline