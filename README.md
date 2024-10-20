# App-de-previs-o-do-tempo-com-Python

import requests
import json
import pprint

accuweatherAPIKey = 'kNQzwpw3eCY9Dy3fyGa9h6GZn3tKgpN1'

def pegarCoordenadas():
  r = requests.get('http://www.geoplugin.net/json.gp')
  if (r.status_code != 200):
      print('Não foi possivel obter a localização. ')
      return None
  else:
    try:
      localização = json.loads(r.text)
      coordenadas = {}
      # Accessing values using keys instead of attributes
      coordenadas['lat'] = localização['geoplugin_latitude']
      coordenadas['lon'] = localização['geoplugin_longitude']
      print( 'lat: ', lat)
      print( 'lon: ', lon)
      return coordenadas
    except:
          return None

def pegarCodigoLocal(lat, lon):
    LocationAPIUrl = "http://dataservice.accuweather.com/locations/v1/cities/geoposition/search?apikey=kNQzwpw3eCY9Dy3fyGa9h6GZn3tKgpN1&q=-23.641669131470724%2C%20-46.6333393154382&language=pt-br"

    r = requests.get(LocationAPIUrl)

    if (r.status_code != 200):
        print('Não foi possivel obter o código do local. ')
        return None
    else:
      try:
        locationResponse = json.loads(r.text)
        infoLocal = {}
        infoLocal['nomeLocal'] = locationResponse['LocalizedName'] + ', ' + locationResponse['AdministrativeArea']['LocalizedName'] + ', ' + locationResponse['Country']['LocalizedName']
        # Accessing values using keys instead of attributes
        infoLocal['codigoLocal'] = locationResponse['Key']
        return infoLocal
      except:
          return None

def pegarCoordenadas():
   r = requests.get('http://www.geoplugin.net/json.gp')
   if (r.status_code != 200):
      print('Não foi possivel obter a localização. ')
   else:
      localização = json.loads(r.text)
      coordenadas = {}
      # Accessing values using keys instead of attributes
      coordenadas['lat'] = localização['geoplugin_latitude']
      coordenadas['lon'] = localização['geoplugin_longitude']
      print( 'lat: ', coordenadas['lat'])
      print( 'lon: ', coordenadas['lon'])
      return coordenadas
def pegarCodigoLocal(lat, lon):
    LocationAPIUrl = "http://dataservice.accuweather.com/locations/v1/cities/geoposition/search?apikey=kNQzwpw3eCY9Dy3fyGa9h6GZn3tKgpN1&q=-23.641669131470724%2C%20-46.6333393154382&language=pt-br"

    r = requests.get(LocationAPIUrl)

    if (r.status_code != 200):
        print('Não foi possivel obter o código do local. ')
    else:
      locationResponse = json.loads(r.text)
      infoLocal = {}
      infoLocal['nomeLocal'] = locationResponse['LocalizedName'] + ', ' + locationResponse['AdministrativeArea']['LocalizedName'] + ', ' + locationResponse['Country']['LocalizedName']
      # Accessing values using keys instead of attributes
      infoLocal['codigoLocal'] = locationResponse['Key']
      return infoLocal

def pegarTempoAgora(codigoLocal, nomeLocal):

    CurrentConditionsAPIUrl = "http://dataservice.accuweather.com/currentconditions/v1/" + codigoLocal + "?apikey=" + accuweatherAPIKey + "&language=pt-br"
    r = requests.get(CurrentConditionsAPIUrl)
    if (r.status_code != 200):
        print('Não foi possivel obter o codigo do local. ')
    else:
        CurrentConditionsResponse = json.loads(r.text)
        infoClima = {}
        infoClima['textoClima'] = CurrentConditionsResponse[0]['WeatherText']
        infoClima['temperatura'] = CurrentConditionsResponse[0]['Temperature']['Metric']['Value']
        infoClima['nomeLocal'] = nomeLocal
        return infoClima

    r = requests.get(CurrentConditionsAPIUrl)
    if (r3.status_code != 200):
        print('Não foi possivel obter o codigo do local. ')
    else:
        CurrentConditionsResponse = json.loads(r.text)
        infoClima = {}
        infoClima['textoClima'] = CurrentConditionsResponse[0]['WeatherText']
        infoClima['temperatura'] = CurrentConditionsResponse[0]['Temperature']['Metric']['Value']
        infoClima['nomeLocal'] = nomeLocal
        return infoClima

coordenadas = pegarCoordenadas()
local = pegarCodigoLocal(coordenadas['lat'], coordenadas['lon'])
climaAtual = pegarTempoAgora(local['codigoLocal'], local['nomeLocal'])

print('Clima atual em: ' + climaAtual['nomeLocal'])
print(climaAtual['textoClima'])
print('Temperatura: ' + str(climaAtual['temperatura']) + '\xb0' + 'C')
