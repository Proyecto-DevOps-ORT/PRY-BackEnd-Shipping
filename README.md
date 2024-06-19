# Payments Service Example

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