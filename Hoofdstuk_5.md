# Hoofdstuk 5: WebStorage

## **Cookies**
>### **Overzicht**
Cookies worden meestal ingesteld door een webserver. Vervolgens voegt de browser ze automatisch toe aan (bijna) **elk request** op **hetzelfde domein**.


- Ingebouwde manier om kleine tekstbestanden **heen en weer te sturen** tussen de **browser en de server**
- Servers kunnen gebruik maken van de inhoud van deze cookies om informatie over de gebruiker bij te houden over **verschillende webpagina's** heen.

>### **Sequentie**
Voorbeeld:
1. De client vraagt aan de server de pagina op, hierbij worden **cookies** meegestuurd in de **header** van het request.
2. De server merkt op dat de taal nog niet gekozen is, er is namelijk **geen language cookie**.
3. De server geeft een pagina terug waarop je de taal kan kiezen.
4. De gebruiker kiest de taal en er wordt een cookie opgeslagen voor het **domein** van de website.
5. De volgende pagina die de client zal opvragen, zal de cookie meesturen waardoor de taal niet opnieuw gekozen moet worden.


>### **Geldigheid**
Een cookie blijft bestaan, zelfs als je de tab of browser sluit. De cookie kan je verwijderen door:

- Manuele acties te nemen
    - `CTRL` + `SHIFT` + `DEL` in Chrome of Edge in te geven en alle cookies te verwijderen
    - `F12` - Application - Cookies en daar te zoeken naar de cookie.
- Automatisch door de browser 
    - Indien de vervaldatum bereikt is.

>### **Structuur**
Een cookie is gevormd door key-value pairs gescheiden door **;**

`"user=John; path=/; expires=Tue, 19 Jan 2038 03:14:07 GMT"`

Cookies hebben verschillende opties, veel daarvan zijn belangrijk en moeten worden ingesteld.

Opties:
```
path
domain
expires, max-age
secure
httpOnly
```

>### **Client-Side**
We kunnen naar `document.cookie` schrijven. Maar het is geen data eigenschap, het is een accessor (`getter`/`setter`). Een schrijfoperatie naar `document.cookie` werkt alleen de cookies bij die erin worden genoemd, maar **muteert geen andere cookies**.
- Cookie met naam "taal" en waarde "NL" instellen:
```javascript
document.cookie = "taal=NL";
```
- verwijderen van een cookie (max-age 1 seconde in het verleden zetten)
```javascript
document.cookie = "taal=;max-age=-1"
```
Om een bepaalde cookie te vinden, kunnen we `document.cookie` splitsen door ; en dan de juiste naam vinden. We kunnen hiervoor een reguliere expressie of array-functies gebruiken.
- geeft alle cookies terug:
```javascript
document.cookies.split(";").forEach((part) => {
    const arrPart = part.split("=");
    console.log(`${arrPart[0]} --> ${arrPart[1]}`);
})
```
>### **Encodering**
Technisch gezien kunnen name en waarde alle karakters hebben, om de geldige opmaak te behouden (spaties) moeten ze worden escaped met behulp van de ingebouwde `encodeURIComponent` functie.
- Encodering van de key-value pairs:
```javascript
const name = "my name"; //let op de spatie
const value = "John Smith";

const encodedName = encodeURIComponent(name);
const encodedValue = encodeURIComponent(value);

//encodeerd cookie als my%20name=John%20Smith
document.cookie = encodedName + "=" + encodedValue;
```
>### **Caveats**

- Het naam=waarde paar, na encodeURIComponent, mag niet hoger zijn dan **4kb**. We kunnen dus niets groots opslaan in een cookie.
- De webserver gebruikt een Set-Cookie header om een cookie in te stellen, waarbij de httpOnly waarde ingesteld wordt. Deze optie **verbiedt elke JavaScript toegang** tot de cookie. We kunnen zo'n cookie niet zien of manipuleren met behulp van document.cookie.
- Elke cookie wordt heen en weer gestuurd voor **elk** HTTP(s) request
- GDPR

>### **GDPR**
Er is een wetgeving in **Europa** die **G**eneral **D**ata **P**rotection **R**egulation(GDPR) heet en die een reeks regels afdwingt voor websites om de **privacy** van de gebruikers te respecteren. En een van die regels is om een expliciete toestemming te eisen voor het traceren van cookies van een gebruiker.
- Dit gaat allen over het **traceren** | **identificeren** | **autoriseren** van cookies
    - Bij een cookie die allen wat informatie opslaat, maar de gebruiker niet traceert of identificeert, ben je vrij om dat te doen

## **WebStorage**

>### **Definitie**
- De webstorage objecten localStorage en sessionStorage maken het mogelijk om key/value pairs op te slaan in de browser.
- Interessant is dat de data een **pagina refresh overleeft** (`sessionStorage`) en zelfs een volledige herstart van de browser (`localStorage`).

>### **Verschil met cookies**

- Data wordt **niet** bij elk request verstuurd.
- Kan meer data stockeren, maar is **geen databank** zoals SQL.
- De **server** kan de **data niet manipuleren**, alles blijft op de client.
- **Per domein**(lees:website), de ene website kan de data van een andere dus niet muteren.
- Data is toegankelijk voor de gebruiker, bij cookies kan het zijn dat de server de cookie zet en is dus niet leesbaar door httpOnly

>### **Localstorage**

- Beschikbaarheid van data over **meerdere tabbladen/vensters** van **hetzelfde domein**.
- Data **blijft beschikbaar** als het venster, tabblad of browser sluit.

Methodes:
```javascript
//DATA OPSLAAN
localStorage.setItem("test", 1);    //beide worden als string opgeslagen

//DATA OPHALEN
localStorage.getItem("test");       //geeft 1 terug
//DATA VERWIJDEREN
localStorage.removeItem("test");    //key "test" en value "1" worden verwijdert
//ALLE DATA VAN DIT DOMEIN VERWIJDEREN
localStorage.clear();
```

Alle key-value pairs doorlopen:
```javascript
//ALS EEN ARRAY
for (let i = 0; i < localStorage.length; i++) {
  let key = localStorage.key(i);
  alert(`${key}: ${localStorage.getItem(key)}`);
}
//ALS EEN OBJECT
for (let key in localStorage) {
    alert(key); //geeft getItem, setItem en buil-in fields terug
}
//ALS EEN OBJECT ZONDER BUILT-IN FIELDS
for(let key in localStorage) {
  if (!localStorage.hasOwnProperty(key)) {
    continue;   // skipt keys zoals "setItem", "getItem" etc
   }
  alert(`${key}: ${localStorage.getItem(key)}`);
}
```

Alle keys en uitprinten doorlopen:
```javascript
//ALS EEN OBJECT VIA OBJECT.KEYS ZONDER BUILT-IN FIELDS
let keys = Object.keys(localStorage);
for(let key of keys) {
  alert(`${key}: ${localStorage.getItem(key)}`);
}
//VIA DE OBJECT.ENTRIES METHODE, KEY-VALUE PAIRS DOORLOPEN
for (let [key, value] of Object.entries(localStorage)) {
  console.log(`${key}: ${value}`);
}
```

>### **Sessionstorage**
- Beschikbaarheid per tabblad/venster, wordt niet gedeeld met een ander venster of tabblad
- Data is verdwenen na het sluiten van het venster of tabblad.
- Synthax is gelijkaardig aan `localStorage`

>### **Object**
Zoals reeds aangegeven is het enkel mogelijk om het datatype string op te slaan. Het datatype Object kan je dus niet -op deze manier- opslaan.
- Standaard wordt de toString() methode gebruikt, dit zorgt ervoor dat [object Object] als string wordt opgeslagen.
```javascript
const myObject = {name:'John', age:12, nationality:'Belgium'};
localStorage.setItem('user', myObject);
alert(localStorage.getItem('user')); // [object Object]
```
Een mogelijke oplossing voor dit probleem zou de volgende aanpak kunnen zijn, hierdoor wordt het object wel correct opgeslagen.
```javascript
let user = {
  name: "John",
  age: 30,
  toString() {
    return {name: "${this.name}", age: ${this.age}};
  }
};
```
***Maar... "this can become a maintenance nightmare", want door het ontwikkelen worden er vaak eigenschappen toegevoegd en kan dit al snel vergeten worden.***

## **JavaScript Object Notation**
>### **Definitie**
*snap ik zelf niet zo goed, voor meer uitleg klik [hier](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON).*

De JSON (JavaScript Object Notation) is een **algemeen formaat** om waarden en objecten weer te geven. Het wordt beschreven zoals in de RFC 4627-standaard. Aanvankelijk werd het gemaakt voor JavaScript, maar veel andere talen hebben ook bibliotheken om er mee om te gaan. Het is dus gemakkelijk om JSON te gebruiken voor **data-uitwisseling** als de client JavaScript gebruikt en de server geschreven is in Ruby/PHP/Java/C#/**Whatever**.

Voorbeeld syntax
```javascript
JSON.parse(text[, reviver])
JSON.stringify(value[, replacer[, space]])
```