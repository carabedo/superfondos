# superfondos

## lambda 

Usamos una lambda para el scraping de los datos.

Creamos una funcion lambda, que utilice python como runtime.

Ahora escribimos el codigo de nuestra funcion:

```python
import json
import requests as rq
from bs4 import BeautifulSoup
import pandas as pd

def lambda_handler(event, context):
    # TODO implement    
    data=getFondos()    
    return {
        'statusCode': 200,
        'body': json.dumps(data)
    }

def getFondos():
    resp=rq.get('https://www.santander.com.ar/ConectorPortalStore/Rendimiento')
    soup = BeautifulSoup(resp.content)
    rows=soup.find('table').find_all('tr')
    filas=[]
    for row in rows:
      try:
        fila=[x.text.strip() for x in row.find_all('td')]
        if len(fila) > 3:
          filas.append(fila)
      except:
        pass
    data=pd.DataFrame(filas)
    data.drop(1, inplace=True, axis=1)
    data.drop(2, inplace=True, axis=1)
    data.columns=[x.text for x in rows[2].find_all('th')]
    return [data['Fondo'].tolist(),data['Variacion Diaria'].apply(tonum).tolist(),data['Valores a la Fecha'].str.replace('.','').str.replace(',','.').astype('float').tolist()]

def tonum(x):
  x=x.replace(',','.')
  x=x.replace('(','-')
  x=x.replace(')','')
  return float(x)
```
Cuando queremos testear nuestra funcion vemos que no puede importar las librerias, por ejemplo para importar `requests` hay que agregar una layer: `arn:aws:lambda:us-east-1:770693421928:layer:Klayers-python38-requests:28`

### layers

En este repo esta la [api](https://github.com/keithrozario/Klayers) con algunas layers para las librerias mas comunes, en nuestro proyecto necesitamos (requests, bs4 y pandas)

## triggers

Creamos un trigger para scrapear diariamente los datos

## dynamoDB 

Guardamos los datos en dynamoDB

## api

Usamos otra lambda para generar una api

## streamlit 

Usamos streamlit para generar una visualizacion y prediccion de los datos



