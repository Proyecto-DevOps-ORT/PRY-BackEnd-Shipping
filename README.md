
# Shipping Service


Este proyecto es un ejemplo de un servicio de shipping utilizando java y Docker.

## Construir imagen
```
docker build -t shipping-service-example:1 .
```
## Crear contenedor
```
docker run --name shipping-service-example-container -d  -p 8083:8080 shipping-service-example:1
```
## Obtener Ip del contenedor
```
docker inspect shipping-service-example-container
```
=> IPAddress": "172.17.0.4"

### Verificacion de app en POSTMAN

- Request tipe: GET
- URL: http://localhost:8083/shipping/a

    Resultado:
    ```
    {
    "status": "Delivered",
    "id": "a"
    }
    ```

    Resultados esperados: 
    ```
    a => Delivered
    b => In transit
    c => Preparing
    ```

# Remoto

## 01: Pre-requisites
- Tener instalado la AWS CLI.
- Configurar las credenciales de la cuenta de academy en el archivo `~/.aws/credentials`.

## 02: Crear ECR repository
- Crear un repo de ECR por la AWS console.
- Repository Name: back-end
- Dejar los demas valores por defecto.

## 06: Create Docker Imaage, tag y push a ECR

EN AWS voy a: 

Amazon ECR => Registropivado=>Repositorios=>pry-backend-shipping


Click en => Ver comandos de envio

Siga los siguientes pasos a fin de autenticar y enviar una imagen a su repositorio. Para obtener métodos de autenticación de registro adicionales, incluido el ayudante de credenciales de Amazon ECR, consulte Autenticación del registro .
Retrieve an authentication token and authenticate your Docker client to your registry. Use the AWS CLI:

```
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 317097728802.dkr.ecr.us-east-1.amazonaws.com
```
>>Nota: Si recibe un error al utilizar AWS CLI, asegúrese de tener instaladas las últimas versiones de AWS CLI y Docker.

Cree una imagen de Docker con el siguiente comando:

```
docker build -t pry-backend-shipping .
```

Cuando se complete la creación, etiquete la imagen para poder enviarla a este repositorio:

```
docker tag pry-backend-shipping:latest 317097728802.dkr.ecr.us-east-1.amazonaws.com/pry-backend-shipping:latest
```
Ejecute el siguiente comando para enviar esta imagen al repositorio de AWS recién creado:

```
docker push 317097728802.dkr.ecr.us-east-1.amazonaws.com/pry-backend-shipping:latest
```
## 06: Create Docker Image locally TEMPLATE

- Crear la imagen de Docker localmente.

Template
```
docker build -t <AWS_ACCOUNT_ID_NUMBER>.dkr.ecr.<REGION>.amazonaws.com/back-end-<Servic name>:1.0.0 . 

o lo que es lo mismo:

docker build -t <URI del repo ECR>-<Service name>:1.0.0 .
```
En nuestro caso
```
docker build -t 317097728802.dkr.ecr.us-east-1.amazonaws.com/back-end-shipping-service-example:1.0.0 . 
```

- Validar que la imagen funciona localmente.
```
docker run --name aws-ecr-back-end-shipping -p 8090:8080 --rm -d 317097728802.dkr.ecr.us-east-1.amazonaws.com/back-end-shipping-service-example:1.0.0
```

## 07: Push Docker Image to AWS ECR
- Pushear la imagen de docker al ECR generado anteriormente.

### Importante paso previo que tuve que hacer en ubuntu

   ```
   gpg --full-generate-key
   ```
- kind of key: 1 (RSA and RSA (default))
- Keysize: 1024 (entre 1024 y 4096)
- Key is valid for?: 0 (con 0 no vence nunca)
- Y
- Real name: dockerKey (aca tendria que haber puesto mi nombre creo)
- Email address: sdavid4815@gmail.com
- Letra O para Okay



**AWS CLI Version 2.x**

Login template
```
aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-ecr-repo-url>
```

   o  lo mismo pero mas especifico
   
```
aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/aws-ecr-nginx
```
Push a ECR template
```
docker push <AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/aws-ecr-nginx:1.0.0
```
Login con mis datos
```
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 317097728802.dkr.ecr.us-east-1.amazonaws.com
```
Push con mis datos
docker push 317097728802.dkr.ecr.us-east-1.amazonaws.com/back-end-shipping-service-example:1.0.0


>Nota: En caso de tener problema con el docker login, utilizar el siguiente comando: `docker login -u AWS -p $(aws ecr get-login-password --region <REGION>) <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com` (NO TUVE QUE USAR ESTO)

## REMOTO AWS IPS
- **Payments Service**: `172.17.0.2:8080`
- **Shipping Service**: `44.195.69.200:8080`
- **Products Service**: `3.89.24.89:8080`

- **Orders service**: `44.195.69.200`


---
---
---
---
***

Agrego Github Actions a Shippping

1. Crear carpeta ".github" en directorio raiz del proyecto
2. Dentro de .github crear carpeta "workflows"
3. Crear arvhivo ".yml" en el que se agregara el workfolow para CI/CD. Nombre del archivo "cd-cd.yml"
4. 


---
---
---
---
***


# Informacion util

## AWS_ACCOUNT_ID_NUMBER
El AWS_ACCOUNT_ID_NUMBER es un identificador único asociado a tu cuenta de AWS. Puedes obtenerlo de varias maneras:

 - Consola de AWS:

    Ingresa a la consola de AWS en tu navegador.
    En la parte superior derecha de la página, haz clic en tu nombre de cuenta o en "Mi cuenta de AWS".
    En la sección "Información de la cuenta", encontrarás tu número de cuenta de AWS.
    AWS CLI:

- AWS CLI:

    Si tienes configurado AWS CLI en tu máquina local, puedes ejecutar el siguiente comando para obtener el ID de cuenta:
   
    ```
    aws sts get-caller-identity --query Account --output text
    ```
    Este comando devolverá el ID de cuenta asociado con las credenciales que estén configuradas en tu entorno local.
## REGION

- AWS Management Console:

    Ingresa a la consola de AWS en tu navegador.
    En la esquina superior derecha, selecciona la región actual (por ejemplo, "Norte de Virginia (us-east-1)").
    Esta es la región donde actualmente estás trabajando en la consola de AWS.
- AWS CLI:

    Si tienes instalado AWS CLI y configurado correctamente, puedes usar el siguiente comando para ver la región configurada:
    ```
    aws configure get region
    ```
    Este comando te mostrará la región que está configurada en tu perfil de AWS CLI.
- AWS Management Console (ECR):

    Ve a la consola de AWS.
    Busca y selecciona "ECR" (Elastic Container Registry) en la lista de servicios.
    En la barra de navegación izquierda, verás una lista de tus repositorios de ECR.
    La región donde están ubicados tus repositorios de ECR se muestra en la URL de la consola ECR. Por ejemplo, us-east-1.
## Instalar y Usar AWS CLI video 
https://www.youtube.com/watch?v=_zMCdUndHy0&ab_channel=Code4Humans

https://www.udemy.com/course/decodingdevops/learn/lecture/32045686#overview



## Instalación de la CLI de AWS

**1. Instalación de la CLI de AWS:**

**Pasos detallados:**

1. **Iniciar terminao:**
   - Abre una terminal en tu máquina.

2. **Descargar el instalador de la AWS CLI:**
   - Ejecuta el siguiente comando para descargar el instalador:
     ```bash
     curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
     ```

3. **Descomprimir el archivo descargado:**
   - Ejecuta el siguiente comando para descomprimir el archivo:
     ```bash
     unzip awscliv2.zip
     ```

4. **Instalar la AWS CLI:**
   - Ejecuta el siguiente comando para instalar la CLI:
     ```bash
     sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin
     ```

5. **Verificar la instalación:**
   - Verifica que la CLI se haya instalado correctamente ejecutando:
     ```bash
     aws --version
     ```
   - La salida debería ser similar a `aws-cli/2.0.41 Python/3.7.4 Darwin/20.6.0 exe/x86_64`.

**2. Configuración de la CLI de AWS:**

**Pasos detallados:**

1. **Obtener credenciales de AWS:**
   - Accede a la consola de AWS 

        => AWS Details => Show

        https://awsacademy.instructure.com/login/canvas
        
        y navega a "AWS Details" para obtener `aws_access_key_id`, `aws_secret_access_key` y `aws_session_token`.

  


2. **Configurar la CLI:**
   - Ejecuta el siguiente comando y sigue las instrucciones para ingresar las credenciales:
     ```bash
     aws configure
     ```
   - Proporciona los siguientes detalles:
     ```bash
     AWS Access Key ID [None]: <aws_access_key_id>
     AWS Secret Access Key [None]: <aws_secret_access_key>
     Default region name [None]: us-east-1
     Default output format [None]: yaml
     
     ```
     Verificar credenciales: 
     ```
     ls ~/.aws
     ```
     En esa ruta estan la carpeta config y credentials
     Verificar que usuario estoy usando
     ```
     aws sts get-caller-identity
     ```
3. **Agregar el AWS Session Token:**
   - Ejecuta el siguiente comando para agregar el `aws_session_token`:
     ```bash
     aws configure set aws_session_token <aws_session_token>
     ```

4. **Verificar la configuración:**
   - Ejecuta el siguiente comando para verificar que las credenciales se ingresaron correctamente:
     ```bash
     aws s3 ls
     ```
   - La salida debería mostrar una lista con los buckets S3 que tenenos en AWS
