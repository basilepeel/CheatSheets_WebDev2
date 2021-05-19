# **Hoofdstuk 8: Ajax - fetch API**

## **Asynchronous JavaScript and XML**

> ### **Gebruik van AJAX**
- Asynchrone requests zorgen ervoor dat meerdere acties op hetzelfde moment kunnen afgehandeld worden (opvragen details en afbeeldingen).
- Requests van browsers worden sneller uitgevoerd.
- Enkel het gedeelte van de pagina dat verandert, wordt aangepast.
- Server verkeer is beperkt – heel voordelig bij applicaties voor mobile devices.


## **Ajax en callback**

> ### **Ajax - XHR object - methodes**
Configureren van het XHR object:
```javascript
.open
```


- HTTP request method: GET – POST – HEADER – PUT – DELETE (zie netwerken en webapplicaties 3 en 4)

- URL: de url waar het request naar verstuurd wordt (het adres). Dit kan een tekst, json, xml bestand zijn. Het kan ook een server-side script zijn (C# - PHP - …) die het request verwerkt.

- Asynchronous: boolean die aangeeft of het request al dan niet asynchroon is. Niet verplicht – default true –

- **Username – password**: voor eventuele authenticatie. Beide zijn niet verplicht.
```javascript
open(method, url, asynchronous, userName, password)
```
**Versturen van het request en data (als parameter)** naar de server:
```javascript
.send
```
**Properties vh XHR object**:
- status van het object:
```javascript
readyState
```
- **Http status** die door de server is teruggegeven
    - 200: OK
    - 404: Page not found
```javascript
status
```
- **Callbackfunctie kopppelen** (die elke keer als de readyState van waarde wijzigd uitgevoerd wordt):
```javascript
onreadystatechange
```
- Server geeft data terug als **string** (kan ook JSON string zijn):
```javascript
responseText
```
- Server geeft data terug als **xml**:
```javascript
responseXML
```


## **Ajax en promises**

> ### **Ajax en promises**
- In plaats van een callback function te koppelen aan het readystatechange event, maken we een promise aan.
- De promise voert het Ajax request uit. 
- Afhankelijk van het resultaat van het request krijgen we een `resolve` (promise succesvol afgehandeld) of een `reject` (niet succesvol).

Voorbeeld:
slide 25 & 26


## **Fetch API**

> ### **Fetch API**
Om een Ajax request uit te voeren moet er steeds dezelfde code gebruikt worden:
- XHR object aanmaken.
- XHR object configureren met open method:
    - http request methode (GET, POST, PUT, DELETE)
    - url
    - …
- `onreadystatechange` event listener:
    - Koppelt een callback functie aan dit event.
    - Deze functie zal uitgevoerd worden van zodra de status van het request verandert.
- XHR object versturen naar server met send method:
- Verstuurt het request naar de server.
- Data kunnen als parameter naar de server gestuurd worden.
Om dit op een meer gestructureerde manier en compacter te doen is de fetch API ontwikkeld:
https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API

The Fetch API is vergelijkbaar met de werking van XHR.
- De `fetch()` methode heeft één verplicht argument, het ‘path’ (vaak url) naar de `resource` die je wil ophalen.
- Deze methode retourneert een Promise die het antwoord zal verwerken van het `request`, of dit succesvol is of niet.
- Eenmaal het antwoord is ontvangen, zijn er een aantal mogelijkheden om het antwoord (body) te verwerken:
    - `Body.json()`: retourneert een Promise dat de body parsed naar JSON
    - `Body.text()`: retourneert een Promise dat de body parsed naar text (UTF-8)

## **Uitgebreid voorbeeld: Countries of the World**
zie slides vanaf slide 32