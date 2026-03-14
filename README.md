# API Salary Prediction

Este proyecto expone una API HTTP en AWS (Lambda + API Gateway) que invoca un endpoint de Amazon SageMaker para predecir el sueldo a partir de los años de experiencia.

## Arquitectura

- API Gateway: recibe solicitudes POST en /predecir/sueldo
- AWS Lambda: handler SalaryPrediction.lambda_handler
- SageMaker Endpoint: salary-linear (region us-east-1)

## Prerrequisitos

- Cuenta AWS con permisos para Lambda, API Gateway, IAM y SageMaker Runtime
- Serverless Framework v4 instalado

## 1) Configurar credenciales AWS

./aws/credentials

## 2) Login y Desplegar la API

Desde la carpeta del proyecto, ejecuta:

serverless login
serverless deploy

Al finalizar, Serverless mostrara la URL del endpoint HTTP creado.

## 3) Obtener la URL del endpoint

Busca la ruta POST /predecir/sueldo.

## 4) Probar con Postman

En este repositorio se incluye la coleccion:

- SalaryPrediction.postman_collection.json

Pasos:

1. Importa la coleccion en Postman.
2. Crea una variable de coleccion llamada baseUrl con el dominio de API Gateway, por ejemplo:
   - https://abc123.execute-api.us-east-1.amazonaws.com
3. Ejecuta la request Predict Salary.

### Body esperado

La API espera JSON con esta estructura:

{
  "years_of_experience": "5"
}

Nota: actualmente el codigo envia years_of_experience como contenido CSV al endpoint de SageMaker.

## 6) Comando de prueba con curl (opcional)

curl -X POST "https://TU_BASE_URL/predecir/sueldo" \
  -H "Content-Type: application/json" \
  -d '{"years_of_experience":"5"}'

## Respuesta esperada

Ejemplo:

{
  "statusCode": 200,
  "salary_prediction": "12345.67"
}

## Solucion de problemas

- Error de permisos (IAM): revisa el role configurado en serverless.yml.
- Error 502/500: revisa logs de Lambda en CloudWatch.
- Endpoint de SageMaker no encontrado: valida nombre salary-linear y region us-east-1.
- Si no aparece years_of_experience en Lambda, revisa el formato del body enviado.

## Limpieza de recursos

Para eliminar lo desplegado:

serverless remove
