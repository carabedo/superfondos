# superfondos

## lambda 

Usamos una lambda para el scraping de los datos

Para importar la libreria requests hay que agregar una layer: `arn:aws:lambda:us-east-1:770693421928:layer:Klayers-python38-requests:28`

## triggers

Creamos un trigger para scrapear diariamente los datos

## dynamoDB 

Guardamos los datos en dynamoDB

## api

Usamos otra lambda para generar una api

## streamlit 

Usamos streamlit para generar una visualizacion y prediccion de los datos



