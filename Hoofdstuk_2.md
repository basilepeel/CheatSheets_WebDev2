# **Hoofdstuk 2: Objecten en Functies**


## **Objecten**

>### **Object aanmaken**
- Constante naam met properties bestaande uit key-value pairs.
    - Groepeer properties tussen {}
    - Scheid properties met ,
    - scheid naam en waarde van propertiy met :

```javascript
const myAvatar = {
    "name avatar": "Bob",
    points: 20,
    gender: "male",
    hair: { color: "black",  cut: "punk"}
};
```

>### **Waarde van property uitlezen**
- Arraynotatie: objectnaam[naamProperty]
    - Kan variabelen met spaties lezen
```javascript
const name = myAvatar["name avatar"];
```
- puntnotatie: objectnaam.naamProperty
```javascript
const points = myAvatar.points;
```

> ### **Waarde van een property wijzigen**
- Met arraynotatie
```javascript
myAvatar["points"] = 50;
```
- Met puntnotatie
```javascript
myAvatar.points = 50;
```

> ### **Een property toevoegen**
- Met arraynotatie
```javascript
myAvatar["age"] = 19;
```
- Met puntnotatie
```javascript
myAvatar.age = 19;
```

> ### **Een property verwijderen**
- Met arraynotatie
```javascript
delete myAvatar["gender"];
```
- Met puntnotatie
```javascript
delete myAvatar.age;
```

> ### **Overlopen van alle properties**
```javascript
for (let key in myAvatar) {
    console.log(`${key}: ${myAvatar[key]}`);
}
```
Uitvoer:
```
name: bob
points: 20
gender: male
hair: [object Object]
```

> ### **Alle keys van properties in een array stoppen**
```javascript
const keys = Object.keys(myAvatar);
```

> ### **Object destructuring**
- Manier om waarden van properties in variabelen te stoppen
    - Voor gender wordt hier de variabele sex gemaakt
```javascript
const {name, points, gender: sex} = myAvatar;
```
- indien de declaratie van de variabelen en de destructuring gescheiden zijn ronde haken nodig
```javascript
let name, points;
({name, points} = myAvatar);
```
- indien een property niet bestaat krijg je de waarde **undefined**
```javascript
const {gender, weight} = myAvatar;
```
- je kan **default waarden** voorzien, deze worden gebruikt bij undefined
```javascript
const {gender = "female", weight = "63"} = myAvatar;
```
- je kan **genest** werken
```javascript
const {name, hair: {color}} = myAvatar;
```
- je kan object en array destructuring **combineren**
```javascript
const myAvatar = {
    "name avatar": "Bob",
    points: 20,
    gender: "male",
    hair: { color: "black",  cut: "punk"},
    features: ["beard", "sunglasses", "smile"]
};

const {name, hair: {color}, features: [first, , third]} = myAvatar
```


## **Functies**

> ### **Declaratie**
- **function** keyword
- **Naam** van de functie
- Lijst van **parameters** voor de functie
    - tussen ronde haakjes gescheiden door komma's
- Javascript **statements**

```javascript
function sayHi(name) {
    return `Hi, my name ${name}`;
}
```

> ### Functies **aanroepen**
- gebruik de naam van de functie gevolgd door de argumenten tussen ronde haakjes
```javascript
console.log(sayHi("Ann"))
```
Uitvoer:
```
Hi, my name is Ann
```
- Een `return` statement stop verdere uitvoering van de functie en retourneert de waarde naar de aanroeper van de functie
    - `return` kan overal in de functie geplaatst worden
    - `return` kan meerdere keren in de functie gebruikt worden
    - Als de return niet bereikt wordt, retourneert de functie undefined
- Veranderingen in de function body aan parameters van een **primitief type** zijn niet zichtbaar in de scope van de aanroeper
- Verandering aan de properties van een object van een **object type** parameter zijn wel zichtbaar in de scope van de aanroeper
- Wanneer geen argumenten meegeven zijn, worden deze als **undefined** beschouwt
```javascript
function dyeHair(avatar, color) {
    avatar.hair.color = color;
}
dyeHair(myAvatar);
concole.log(`myAvatar.hair.color = ${myAvatar.hair.color}`);
//print -> myAvatar.hair.color = undefined
```
- Je kan **default waarden** voorzien die bij lege argumenten gebruikt worden
```javascript
function dyeHair(avatar, color = "green") {
    avatar.hair.color = color;
}
dyeHair(myAvatar);
concole.log(`myAvatar.hair.color = ${myAvatar.hair.color}`);
//print -> myAvatar.hair.color = green
```

>### Rest parameters
- De rest parameter stelt onbeperkt aantal parameters voor als een array
- De rest parameter wordt voorafgegaan door `...`
```javascript
function f(a, b, ...otherArgs) {
    for (let value of otherArgs) {
        console.log(value);
    }
}

f(1, 2, 3, "four", 5);
```
Uitvoer:
```
3
four
5
```
- Je kan gebruik maken van array destructuring in de parameterlijst
```javascript
function f(a, b, ...[c, d, e, f]) {
    console.log(a);
    console.log(b);
    console.log(c);
    console.log(d);
    console.log(e);
    console.log(f);
}

f(1, 2, 3, "four", 5);
```
Uitvoer:
```
1
2
3
four
5
undefined
```

>### Functies zijn first class objects
- kan worden toegekend aan een variabele
- kan de waarde zijn van een property van een object
- kan doorgegeven worden als argument naar functies
- kan een returnwaarde zijn van een functie
-kan aangemaakt worden 'at run-time'

>### Functie expressie
Kent een variabele toe aan een functie
- kan anoniem zijn (de functie heeft geen naam)
- kan deze variabele als parameter gebruiken
```javascript
const dyeHair = function (avatar, color) {
    avatar.hair.color = color;
}

const makeColor = function() {
    return "green";
}

dyeHair(myAvatar, makeColor);
concole.log(`myAvatar.hair.color = ${myAvatar.hair.color}`);
//print -> myAvatar.hair.color = green
```

>### Scope van variabelen

- Variabelen en de parameters, gedeclareerd in een functie, zijn enkel binnen de functie toegankelijk
- **Scope chaining**: de functie heeft ook toegang tot variabelen uit de scope waarin de functie werd gedeclareerd

Voorbeeld:
```javascript
let globalX = "global X";
let cupOfTea = "Camomille";
let outerFunction = function(param1, param2) {
    let outerX = "outer X";
    let cupOfTea = "Fennel"; //verbergt cupOfTea van global scope
    let cupOfCoffee = "Cappuccino";
    let innerFunction = function(param1, param3) { //verbergt param1 van outerFunction
        let innerX = "inner X";
        let cupOfCoffee = "Espresso";
    };
};
```

**Om temporal dead zones te vermijden declareer je locale variabelen bovenaan de functie**

>### Arrow Functies
- Compactere syntax om functie expressies te schrijven (altijd anoniem)
```javascript
const functieNaam = (a, b, c) => a + b + c;
```
Retourneert de som van de parameters a, b en c

## **Objecten en functies**
- Een **methode** van een object is een **property** met als waarde een **functie** (methode en properties *zijn hetzelfde*)
- Een methode van een object kan worden aangeroepen met de punt- of array-notatie

```javascript
const myAvatar = {
    "name avatar": "Bob",
    points: 20,
    gender: "male",
    hair: { color: "black",  cut: "punk"},
    sayHi: function() {
        const title = this.gender === "male" ? "Sir" : "Miss";
        return `Hi, I am ${title} ${this.name}`;
    }
};
console.log(myAvatar.sayHi()); //print -> Hi, I am Sir Bob
```

>### **`this`**
- Binnen de body van een functie is de `this` variabele beschikbaar
    - Als de functie gedefinieerd is in de **global scope** verwijst `this` naar het global object (in de browser is dat `window`)
    - Als `this` gebruikt wordt in ee nmethode van een object verkrijg je een verwijzing naar het parent object waar de methode toe hoort
- Gebruik steeds `this` om binnen een object naar één van zijn properties of methods te verwijzen
-  !!! **Arrow functies** beschikken niet over hun *eigen* `this`, en dit maakt ze **ongeschikt** om als **methodes** gebruikt te worden !!!

>### **Voor- en nadelen een object literal**
- **Eenvoudig** instantie en gebruik van een object
- Geen mogelijkheid om meerdere instanties van eenzelfde  **type** te maken
- Geen afscherming van private en publieke properties/functies. alle code in een object is beschikbaar voor de buitenwereld en kan worden aangepast.
    - gebruik `_` voor lokale variabelen en methodes om deze als *private* te beschouwen.

## **Events**
Een gebruiker interageert met een webpagina via **user interface**. <br>
Een handeling op een HTML element zal resulteren in het afvuren van één of meerde events. Wanneer een event wordt afgevuurd, dan wordt de bijhorende **event handler** aangeroepen.<br>
- Een event handler is een **functie**
>### **Het `window` object**
- Het `window` object is de globale scope in de browser
>### **Het `document` object**
- Het `document` object is  een property van het window object
- Het heeft properties die object representaties van de HTML pagina bevatten (zie [Hoofdstuk 6: DOM](Hoofdstuk_6.md))
```
Elk HTML element van een pagina komt overeen met een javascript object

De attributen van de HTML elementen zijn properties van dit object
```

>### **Voorbeeld `getElementById`**
- Haalt object op met de HTML tag die overeenkomt met het `id`


```html
<body>
    <h1>My avatar</h1>
    <input type="button" id="sayHi" value="sayHi"/>
    ...
    <script src="events.js"></script>
</body>
```
```javascript
const sayHiButton = document.getElementById("sayHi");
sayHiButton.value = "Yebo!";
//value van de button verandert van sayHi naar Yebo!
```

>### **Events triggeren**
- user generated (bv. keyboard/mouse events)
- API generated (bv. animation finished running)
- Event heeft
    - een naam (bv. click, change, load, mouseover, keyup, focus...)
    - een target (het element waarop het event van toepassing is bv. de button met id sayHi) 
- Een event wordt voorzien van een **eventhandler** of **callback functie** (onlcik, onload, onmouseenter, ondbclick)

>### **`load` event**
- Wordt getriggerd bij het opstarten van de pagina
- Best onderaan te plaatsen zodat ander variabelen al geïnitialiseerd zijn
```javascript
function init() {

}

window.onload = init();
```

>### **`addEventListener`**
- Meerdere event handlers instellen voor eenzelfde event
```javascript
cosnt sayHiClicked = function() {
    alert(myAvatar.sayHi());
};

const beClever = function() {
    alert(`I know you clicked on the button that says ${this.value}`);
};

const init = function() {
    const sayHiButton = document.getElementById("sayHi");
    sayHiButton.addEventListener("click", sayHiClicked);
    sayHiButton.addEventListener("click", beClever);
}
```

## **Closures**

*A **closure** is a function, whose return value depends on the value of one or more variables declared outside this function.<br>The function defined in the closure "remebers" the environment in which it was created.*

- closures worden gecreëerd via **inner (geneste) functions**

```javascript
const makeFunc = function() {
    const message = "Sir bob is still around";
    const displayName = function() {
        console.log(message);
    };
    return displayName;
};

const aDisplayFunction = makeFunc();

console.log(aDisplayFunction());
```
- Parameters van de outer functie kunnen deel uitmaken van een closure

## **Exceptions**
Voorbeeld:
```javascript
try {
    throw {
        name: "SomethingWentWrongError",
        message: "Something went wrong. You should fix it"
    };
} catch (e) {
    //handel de exception hier af
    console.log(`Error ${e.name}: ${e.message}`);
} finally {
    //code wordt uitgevoerd wanneer exception voorkomt
}
```
Uitvoer:
```
Error SomethingWentWrongError: Something went wrong. You should fix it
```
>### **`throw`**
- Gooit een excetion object
>### **`try` ... `catch` ... `finally`**
- Vangt de exception op

