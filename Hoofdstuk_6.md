# **Hoofdstuk 6: Document Object Model**

## **Inleiding**

> ### **Wat is DOM?**
Web API

Vanuit een programma kan je een HTML document als volledig object benaderen
- Bouwt een **boomvoorstelling** van het HTML document in het geheugen
- Biedt klassen en methodes (**tree-gebaseerde API**) aan om via code door de boom te navigeren en bewerkingen uit te voeren.

Platform- en taalonafhankelijke API

Ontworpen voor HTML en XML

DOM stelt het HTML Document voor als een boom met nodes.

**Node**:
- Document:
    - DOCUMENT_NODE bevat toegangspoort tot de DOM, stelt volledige document voor
    - Bevat verwijzing naar het `<html>`-element
- Element:
    - ELEMENT_NODE: elk html-element in de boom komt hiermee overeen
-Text:
    - TEXT_NODE: tekst-inhoud van een element. Voorgesteld als child vh element-node

**DOM + javascript**
- DOM manipulatie
- wijzingen in DOM gereflecteerd op webpagina
- Nadelen: Volledige boom wordt in geheugen opgeslagen, daarom vooral bruikbaar bij kleine documenten die moeten gewijzigd worden, niet bruikbaar voor grote documenten

Voorbeeld:
```javascript
<html>
    <head>
        <title>Voorbeeld 1</title>
        <link rel="stylesheet" href="css/urbanus.css" />
    </head>
    <body>
        <div class="main">
            <h1>De avonturen van Urbanus</h1>
            <section id="urbanus_140">
                    <h2>De Fluwelen Grapjas</h2>
                    <img src="images/urbanus_140.jpg" alt="urbanus_140" />
                    <p>Eindelijk erkenning! ...</p>
            </section>
        </div>
    </body>
</html>
```

## **Overzicht voorbeeld 2dehands**
zie slides vanaf slide 9

> ### **De klasse Product**
Properties:
- id
- eigenaar
- postcode
- gemeente
- titel
- omschrijving
- prijs
- categorie
- afbeeldingen
```javascript
class Product {
    constructor(id, eigenaar, postcode, gemeente, titel,
omschrijving, prijs, categorie, afbeeldingen) {
    this._id = id;
    this._eigenaar = eigenaar;
    this._postcode = postcode;
    this._gemeente = gemeente;
    this._titel = titel;
    this._omschrijving = omschrijving;
    this._prijs = prijs;
    this._categorie = categorie;
    this._afbeeldingen = afbeeldingen;
    }
// getters
}
````

> ### **De klasse ProductenRepository**
Repository is centrale plaats waar data opgeslagen wordt + aantal eenvoudige bewerkingen rond deze data implementeren:
- Alle data opvragen
- Specifieke data opvragen adhv bepaalde voorwaarden
- Data toevoegen/verwijderen
- ...
```javascript
class ProductenRepository {
    constructor() { this._producten = []; this.haalProductenOp(); }

// Voegt het product achteraan toe aan de array _producten
voegProductToe(product) { ... }

// Retourneert het product met opgegeven id
geefProduct(id) { ... }

// Retourneert een array met producten behorend tot de opgegeven categorie
geefProductenUitCategorie(categorie) { ... }

// Retourneert een array van strings met unieke categorieën
geefAlleCategorieen() { ... }

// Vul de repository met producten (hard gecodeerd)
haalProductenOp() { ... }
```

> ### **De klasse ProductenComponent**
Lijm tussen ons domein (Product & ProductenRepository) en de HTML-pagina
 - Als de gebruiker iets wijzigt op de html pagina gaat het domein aangesproken worden en de web pagina aangepast worden

ProductenComponent heeft nood aan:
- domeinobject(en), in dit geval ProductenRepository
- DOM, sws toegankelijk via de document property vh global window-object
```javascript
class ProductenComponent {
    
    constructor() { this._productenRepository = new ProductenRepository(); }
// Initialiseer de pagina
initialiseerHtml() {

// Vult de select-list met alle categorieën
categorieenToHtml() { ... }

// Toont overzicht van alle producten voor de opgegeven categorie
productenToHtml(producten) { ... }

// Toont de details van het product met opgegeven id
productDetailsToHtml(product) { ... }
```

> ### **De function `init()`**
In de function `init()` wordt een object van de klasse ProductenComponent geïnstantieerd en wordt de pagina geinitialiseerd.
```javascript
function init() {
    const productenComponent = new ProductenComponent();
    productenComponent.initialiseerHtml();
}
```
`init()` aanroepen als DOM volledig is opgebouwd
```javascript
window.onload = init;
```


## **Voorbeeld 2deHands - dynamisch updaten vd pagina**

> ### **Wijzigen vd DOM - `insterAdjacentHTML()`**
`insertAdjacentHTML()` parset meegegeven **HTML-tekst** + voegt resulterende nodes toe ad DOM structuur op de meegegeven **positie**
```javascript
element.insterAdjacentHTML(position, text);
```
Voorbeeld:
```javascript
const element = document.getElementById('pId');
element.insertAdjacentHTML('afterbegin', '<div>Dit is de toegevoegde div</div>');
```

> ### **Wijzigen vd DOM - `innerHTML`**
`innerHTML`-property ve element: HTML opvragen/wijzigen die zich binnen dat element bevindt
- Merk op dat als je de innerHTML wijzigt je alles die binnen het element aanwezig was overschrijft... 
```javascript
const element = document.getElementById('pId');
element.innerHTML = '<div>Dit is de div! Waar is de rest?</div>'

element.innerHTML = '';
```

> ### **Wijzigen vd DOM - werken met nodes**
Kan zelf expliciet nodes maken en toevoegen aan DOM-tree

**Creatie** van een node
- const anElementNode = `createElement(tagName)`
- const aTextNode = `createTextNode(text)`

**Toevoegen** van een node
- `node.appendChild(newChild)`
    - Voegt een node `newChild` toe na de laatste child-node van node. De nieuw toegevoegde node wordt geretourneerd. 

Nog enkele methodes:
Verwijder child node vd document tree. Verwijderde node wordt geretourneerd(kan dus evt verder gebruikt worden)
```javascript
removeChild(removeChild)
```

Vervang `oldChild` node door `newChild` node, verwijderde `oldChild` node wordst geretourneerd(kan dus evt verder gebruikt worden)
```javascript
replaceChild(newChild, oldChild)
```

Creëert + retourneert exacte kopie.
    - Indien boolean parameter `deepcopy = true` kopie gemaakt vd node zelf en volledige subtree
    -  Indien boolean parameter `deepcopy = false` enkel kopie van de node zelf
```javascript
cloneNode(deepcopy)
```

Werken met attributen:
```javascript
setAttribute(attrName, attrValue)
getAttribute(attrName)
removeAttribute(attrName)
```


Merk op: voor standaard attributen (~attributen herkend oor de browser) kan je ook werken met de **.-notatie**
- Daar class een gereserveerd woord is dien je `.className` te gebruiken ipv 
`.class`

Voorbeeld:
```javascript
const divElement = document.createElement("div");
divElement.setAttribute("id", "divId");
const divTekst = document.createTextNode("Hier is de tekst voor de div...");
divElement.appendChild(divTekst);
element.appendChild(divElement);
```


## **interactiviteit via events**

> ### **Events - interactiviteit**
Als een **andere categorie** wordt geselecteerd dan moet het **overzicht van de producten aangepast** worden

Als **change-event** wordt afgevuurd op selectie #categorie moet:
- Categorie die werd gekozen moet geweten zijn, via `value`-attribuut vh select-elemet
- Juiste producten ophalen via repository: 
```javascript
geefProductenUitCategorie(categorie)`
```

- Productenoverzicht aanpassen 
```javascript
productenToHtml(producten)
```

- Er is op dat moment geen product geselecteerd en dus moet de div `#productDetails` niet getoond worden, dit kan via het `style.display`-attribuut op “none” te zetten

```javascript
document.getElementById('categorie').onchange = () => {
    this.productenToHtml(
        this._productenRepository.geefProductenUitCategorie(
            document.getElementById('categorie').value));
            document.getElementById('productDetails').style.display = 'none';
        }
    }
}
````
Als **click-event** wordt afgevuurd op de div `#productId` moet:
- Details vh product ophalen via de repository
```javascript
geefProduct(id)
```
- div #productDetails aanpassen:
```javascript
productDetailsToHtml(product)
```
Merk op: de lijst van producten wordt dynamisch aangemaakt (~`productenToHtml`), het is dus daar dat we de event handlers gaan definiëren

```javascript
productenToHtml(producten) {
    ...
    producten.forEach((product, index) => {
        const divElement = document.createElement('div');
        divElement.id = product.id;
        ...
        divElement.onclick = () => {
            this.productDetailsToHtml(product);
        }
        ...
    });
}
```


## **Flexibel nodes selecteren & cheatsheet**

- Feeft het eerste element met de opgegeven id
- De zoekbewerking start meestal vanaf het document element, maar je kan evengoed van elk ander element starten
```javascript
getElementById(id)
```

- Retourneert een live HTML collection met alle elementen (0 of meer) met de opgegeven tagname
- Een HTML collection is een array-like object van elementen en voorziet properties en functies om elementen te selecteren in de lijst
```javascript
getElementsByTagName(tagname)
```

- Retourneert een live HTML collection met alle elementen met de opgegeven value als waarde voor het class attribuut
```javascript
getElementsByClassName(value)
```

- Retourneert het eerste element dat voldoet aan de opgegeven CSS selector
```javascript
querySelector(CSS selector)
```

- Retourneert een non live (static) nodelist met alle elementen die voldoen aan de opgegeven CSS selector. 
```javascript
querySelectorAll(CSS selector)
```
<br>

Voorbeeld: als een nieuw product wordt gekozen dan gaan we in de methode productToHtml
- De class `tekstVet` weghalen van het huidig gekozen product
    - Let op: mogelijks is er geen product met class tekstVet, bv. net na het veranderen van de categorie
- De class `tekstVet` toevoegen aan het nieuwe gekozen product
```javascript
// zet, in het overzicht van de producten, het gekozen product in het vetjes
if (document.querySelector(`#overzichtProducten .tekstVet`))
    document.querySelector(`#overzichtProducten .tekstVet`).removeAttribute('class');
    document.querySelector(`#${product.id} p`).setAttribute('class', 'tekstVet');
```