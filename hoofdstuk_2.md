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






```javascript

```