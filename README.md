# Guía Jenkins
El propósito de este repositorio es guiar la implementación de pipelines utilizando Jenkins, esta incluye configuración básica y ejemplos.

# Tabla de contenidos
- [Guía Jenkins](#guía-jenkins)
- [Tabla de Contenidos](#tabla-de-contenidos)
    - [Inicio](#inicio)
      - [Ejecución](#ejecución-y-primera-configuración)
      - [Primera configuración](#primera-configuración)
        - [Plugins recomendados](#plugins-recomendados)
          - [Webhook Generic Trigger](#webhook-generic-trigger)
          - [SSH Agent](#ssh-agent)
        - [Acceso a GitHub](#acceso-a-github)
      - [Pipelines](#pipelines)
        - [Estructura básica](#estructura-básica)
          - [Casos específicos](#casos-específicos)
            - [Pipeline con parámetros](#pipeline-con-parámetros)
            - [Pipeline con webhook](#pipeline-con-webhook)

# Inicio
## Ejecución
Dirijase al siguiente directorio:

```bash
cd ./deployment
```

Una vez dentro, ejecute el docker compose para levantar jenkins.

```bash
docker compose up -d 
```

## Primera configuración
Una vez levantado, podrá acceder a su Jenkins con total normalidad, es importante mencionar que si está de forma local podrá acceder con localhost:8080, si no, una vez configurado el web server acceda a su url configurada.

Para poder iniciar por primera vez, debe acceder a los logs del contenedor.

```bash
docker logs -f deployment-jenkins
```

Dentro, podrá encontrar la clave inicial para poder acceder a Jenkins, así, proceda a crear tanto usuario como contraseña.

Es importante mencionar que el caso ideal para aplicar las siguientes configuraciones, son las siguientes:

```bash  
  Jenkins -> VM -> GitHub
```

Es decir, Jenkins debe poder acceder a la VM y la VM debe poder tener acceso a GitHub.

### Plugins recomendados
Para poder acceder a la máquina virtual, SSH Agent es bastante útil, así, es parte de la primera tanta de plugins necesarios.

Si quisiera implementar pipelines que se activan con un hook, instale Webhook Generic Trigger.

Es recomendado también instalar los plugins sugeridos por Jenkins.

A continuación se detalla una pequeña explicación respecto de los 2 primeros plugins.

#### Webhook Generic Trigger
Este plugin nos permite recibir hooks desde GitHub configurandolo de manera sencilla.

Nos provee una URL donde está hosteado Jenkins donde Github podrá enviar los webhooks, esta tiene la siguiente estructura:

```bash
 http://JENKINS_URL/generic-webhook-trigger/invoke
```
Si se quisiera agregar un Token para mayor seguridad, bastaría con agregar /invoke?token=TOKEN_HERE a la URL, el token se configura en la misma pestaña del plugin.

Dentro del payload que Github envía a través de este Hook se encuentra (entre mucha más):

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

Con esta información, podemos crear parámetros con la siguiente estructura:

```bash
nombre del parámetro: repositoryName
expresión: $.repository.name (esto dice: del payload accede a ->repository->name)
```
Debe seleccionar JSONPath.

Así, podremos utilizarla a conveniencia.

#### SSH Agent
Este plugin nos permite poder conectarnos a una VM a través de un sshagent con credenciales específicas.

Para configurarlo, realice lo siguiente:

  1) Cree sus llaves públicas/privadas en la VM con el siguiente comando.

```bash
ssh-keygen
```

  2) Acceda a: Jenkins -> Manage Jenkins -> Credentials, una vez allí, debe crear una nueva credencial SSH, lo más imporante es poder asignarle un ID para poder acceder a el rápidamente, además, el username que será por el que nos conectaremos vía SSH y por último, la llave privada generada en el paso anterior.

### Acceso a GitHub
Para realizar esto, utilizaremos la llave pública creada en el paso de SSH Agent.

Acceda a la configuración de su cuenta de GitHub, una vez allí, diríjase a SSH and GPG keys y agregue una llave SSH, ahí debe escribir un nombre representativo y además, pegar la llave pública de su VM, esto asegura el acceso a GitHub y no solo eso, la posibilidad de hacer operaciones con git sin la necesidad de autorizar con token/contraseña.

## Pipelines
Una vez asegurada la configuración preliminar, puede continuar a implementar sus pipelines.

### Estructura básica

#### Casos específicos
##### Pipeline con parámetros
##### Pipeline con webhook
