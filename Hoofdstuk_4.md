# **Hoofdstuk 4: Functional Programming met Arrays**


## **Functioneel programmeren**

> ### **Pure functions**
Pure functie is een voorspelbare functie
- Aanroepen met input, dezelfde output
- Geen side-effects

Heeft altijd een `return` statememt

Een **shared state** is elke variabele of object die bestaat in een gedeelde scope of die wordt doorgegeven naar een andere scope
```javascript
// Een gedeelde variabele creëren
let gedeeldeVariabele = 0;

function verhogen(){
    gedeeldeVariabele += 1;
}

function verdubbelen(){
    gedeeldeVariabele *= 2;
}

verhogen();
console.log(gedeeldeVariabele);
verdubbelen();
console.log(gedeeldeVariabele);
```

> ### **Mutable vs Immutable objecten**
Immutable(onveranderlijk): object dat na creatie niet meer gewijzigd kan worden

Mutable(veranderlijk): object dat na creatie wel gewijzigd kan worden

Bij mutatie van een shared state object kan dit onvoorspelbare/ongewenste effecten hebben op het programma  
- zoveel mogelijke onveranderlijke data
```javascript
// Een array maken (muteerbaar)
const hobbies = [
    ‘programmeren’,
    ‘gamen’,
    ‘voetbal’
];

const omgekeerdeHobbies =
    hobbies.reverse();

console.log(omgekeerdeHobbies);
//[‘voetbal’, ‘gamen’, ‘programmeren’]

console.log(hobbies);
//[‘voetbal’, ‘gamen’, ‘programmeren’]


// Een string maken (niet-muteerbaar)
const origineel = "Ik ben niet muteerbaar";

const gewijzigd = origineel.replace("Ik ben
niet muteerbaar", "Ik ben gewijzigd");

console.log(gewijzigd);
// "Ik ben gewijzigd"

console.log(origineel);
// "Ik ben niet muteerbaar"
```

> ### **Side-effects**
Een side effect is iedere verandering aan de toestand van een applicatie die zichtbaar is buiten de opgeroepen functie (behalve de return waarde)
- Aanpassen externe variabele/object
- Loggen nr console
- Schrijven nr scherm/bestand/netwerk
- Oproepen extern process

> ### **Declaratief vs Imperatief**
Imeratief
- **Hoe** programma functioneert
- `flow control`: beschrijving verschillende stappen om resultaat te bereiken
```javascript
const arr = ["een", "twee", "drie"];

function zoek(waarde) {
    for (let i = 0; i < arr.length; i++) {
        if(arr[i] === waarde)
return i;
}
return -1;
}
zoek("twee"); //1
zoek("zes"); //-1
```
Declaratief
- **Wat** programma moet bekomen, zonder te specifiëren hoe
- Beschrijving vd `data-flow`
```javascript
const arr = ["een", "twee", "drie"];

/* We maken gebruik van de indexOf
methode van een array.
Hoe deze te werk gaat maakt ons niet
uit. */

arr.indexOf("twee"); //1
arr.indexOf("zes"); //-1
```

## **Arrays**
```javascript
// De klassieke manier
for (let i = 0; i < fruit.length; i++) {
    console.log(fruit[i]);
}

// Nog een manier met behulp van for-of
for(let element of fruit){
    console.log(element);
}

// orange
// pear
// kiwi
// melon
```

> ### **Callback functies**
Een callback functie, is een functie die wordt uitgevoerd NADAT een andere functie klaar is.
```javascript
function doHomework(subject, callback) {
    console.log(`Starting my ${subject} homework.`);
    callback();
}

function alertFinished(){
    console.log('Finished my homework');
}

doHomework('math', alertFinished);
```

> ### **Map - Filter - Reduce**
`map`: Bewerking toepassen op ieder element vd array + bewerkte kopie vd originele array trg

`filter`: Elementen uit de array die aan bepaalde criteria voldoen halen

`reduce`: Elementen uit de array gebruiken om iets nieuws te berekenen

Verwacht dat er een callback functie meeggeven wordt

Bij iedere iteratie krijgt de functie automatisch een aantal argumenten mee
- `value`: Huidige waarde tijdens de iteratie


- `index`: Huidige index(teller) vd iteratie
- `Array`: Kopie van de hele array
```javascript
arr.map(callbackFunctie);

function callbackFunctie(value, index, array) {
}

//de inline versie met een anonieme functie
arr.map(function(value, index, array) {
});

//de arrow notatie
arr.map((value, index, array) => {});
```

> ### **Filter**
De functie: `Array.prototype.filter(callback(item));`
- De callback neemt een item uit de array als argument en retourneert een booleaanse waarde. Als het true retourneert, wordt het item toegevoegd aan de nieuwe array. Als het false retourneert, wordt het item weggelaten.
- [10, 8, 12, 15, 4].filter(geslaagd)

> ### **Map**
De functie: `Array.prototype.map(callback(item));`
- De callback wordt uitgevoerd op elk item van de array. De geretourneerde waarde is het nieuwe item in de resulterende array.
- [10, 8, 12, 15, 4].map(puntenOp100)

> ### **Reduce**
`Array.prototype.reduce(callback(accumulator, currentValue), initialValue)`
- De callback heeft ten minste twee argumenten. De eerste is de accumulator waarde die is geretourneerd door de laatste iteratie. De tweede is de huidige waarde die wordt herhaald in de array. De geretourneerde waarde wordt doorgegeven als het eerste argument (accumulator) in de volgende iteratie. Het eindresultaat is de geretourneerde waarde in de laatste iteratie
- const punten = [10, 8, 12, 15, 4];
- const gemiddelde = punten.reduce(som, 0)/punten.length
- [10, 8, 12, 15, 4].reduce(aantalGeslaagden, 0)


## **Geavanceerde methodes**
`forEach`: generische functie die over een array itereert en een callback functie aanroept tijdens elke iteratie

> ### **Arrays- geavanceerde methodes**
```javascript
// De klassieke manier van itereren
for (let i = 0; i < fruit.length; i++) {
    console.log(fruit[i]);
}

// Nog een manier met behulp van de geavanceerde methode: forEach
fruit.forEach(function(element) {
    console.log(element);
});
// orange
// pear
// kiwi
// melon

// Hetzelfde maar korter met behulp van arrow functies
fruit.forEach((element) => console.log(element));

// Idem. Als er maar één parameter is moeten er geen ronde haakjes staan
// rond de parameter
fruit.forEach(element => console.log(element));

// De meest algemene vorm van forEach
fruit.forEach((item, index, array) => {
    console.log(`${item} is at index ${index} in ${array}`);
});

// orange is at index 0 in orange,pear,kiwi,melon
// pear is at index 1 in orange,pear,kiwi,melon
// kiwi is at index 2 in orange,pear,kiwi,melon
// melon is at index 3 in orange,pear,kiwi,melon
````
Slide 48!

`find` & `findIndex`
- Retourneert (de index van) het eerste element waarvoor de callback `(~voorwaarde) true` retourneert
- Indien geen enkel element aan de voorwaarde voldoet dan retourneert `find`: 
    - `undefined` & `findIndex: -1`

`sort`
- Callback is een comparator die bepaald hoe elementen zullen gesorteerd worden
- De array wordt **in-place** gesorteerd, sort retourneert de gesorteerde array, **dit is dus geen copy van de originele array**


## **Maps**
The `Map` object **holds key-value pairs**.
- It remembers the **original insertion order** of the keys.
- Any value **(both objects and primitive values)** may be used as either a key or a value.

`Map()` om een nieuwe Map te creëeren
```javascript
const population = new Map();
```
`set(key, value)` voegt key-value pair toe of past bestaand key-value pair aan
```javascript
population.set('Belgium', 11589623);
population.set('Burkina Faso', 3273);
population.set('Iceland', 341243);
population.set('Burkina Faso', 20903273);
```
`set(key, value)` retourneert de aangepaste map, dit maakt method chaining mogelijk
```javascript
population
    .set('Belgium', 11589623)
    .set('Burkina Faso', 20903273);
    .set('Iceland', 341243);
```
`get(key)` retourneert de value van de entry met de opgegeven een key (undefined voor onbestaande key)
```javascript
let country = prompt('Enter name of country.'); population
while (country) {
    alert(`${population.get(country)} people live in ${country}`);
    country = prompt('Enter name of country.');
}
```
**key equality** wordt bepaald adhv `===`
```javascript
const primitive1 = 'abcdef';
const primitive2 = 'abcdef';
console.log(primitive1 === primitive2);
//true

const object1 = [1, 2, 3];
const object2 = [1, 2, 3];
console.log(object1 === object2);
//false
```
`size` hoeveel key-value pairs een map bevat
```javascript
alert(`We have data for ${population.size} countries.`);
```
`has(key)` retourneert een boolean die aangeeft of entry met opgegeven key aanwezig is in de map
```javascript
if (population.has(country))
    alert(`${population.get(country)} people live in ${country}`);
else
    alert(`We have no data for ${country}`);
```
`delete(key)` entry met opgegeven key verwijderen uit een Map
```javascript
if (population.delete(country))
    alert(`The entry for ${country} was deleted`);
else
    alert(` Couldn't not delete ${country}...`);
```
`clear()` een map in 1 keer legen
```javascript
population.clear();
alert(`The map has been cleared. It contains ${population.size} entries...`);
```
`keys()` retourneert een itereerbaar object
- Object bevat alle keys in insertion order
- Over dit object itereren met een `for..of`loop
```javascript
message = 'Keys in our map:\n';
for (const key of population.keys()) {
    message += `${key}\n`;
}
alert(message);
```
`entries()` retourneert een itereerbaar object met alle entries
```javascript
message = 'Entries in our map:\n';
for (const entry of population.entries()) {
    message += `${entry[0]} has ${entry[1]} people.\n`;
}
alert(message);
```
- Tip: maak gebruik van **array destructuring** 
```javascript
for (const entry of population.entries()) {
    message += `${entry[0]} has ${entry[1]} people.\n`;
}

for (const [key, value] of population.entries()) {
    message += `${key} has ${value} people.\n`;
}
```
`forEach(callback)`
```javascript
message = 'Countries with less than 5000000 people:\n'
population.forEach((value, key) => {
    if (value < 5000000)
message += `${key} with ${value} people\n`;
});
alert(message);
```
Constructor `Map()` revisited
- Je kan een array argument gebruiken om een map te creëren met een aantal entries
- In deze array stop je key-value pairs in de vorm [key, value]
```javascript
population = new Map([
    ['Belgium', 11589623],
    ['Burkina Faso', 20903275],
    ['Iceland', 341243],
]);
```

## **Sets**
> ### **Sets**
The `Set` object holds **unique values**.
- It remembers the **original insertion order** of the values.
- Any value **(both objects and primitive values)** may be used.

`Set()` om een nieuwe Set te creëren
```javascript
let viewers = new Set();
```
Of, geef eventueel via een array de initiële waarden op:
```javascript
let viewers = new Set(["tom.antjon@hogent.be", "stefaan.decock@hogent.be"]);
```
`add(value)` als de value nog niet aanwezig is in de set dan wordt value toegevoegd, methode retourneert de al dan niet gewijzigde set
```javascript
viewers
.add('patrick.lauwaerts@hogent.be')
.add('stefaan.samyn@hogent.be')
.add('pieter.vanderhelst@hogent.be')
.add('benjamin.vertonghen@hogent.be’)
.add('patrick.lauwaerts@hogent.be');
```
`size`-property bevat het aantal values in de set
```javascript
alert(`We have now ${viewers.size} unique viewers...`);
```
`has(value)` retourneert een boolean die aangeeft of de opgegeven value aanwezig is in de set
- op dezelfde manier als bij Maps gebruik gemaakt van `===`
```javascript
viewer = prompt('Who do you want to follow up?', 'email');
while (viewer) {
    alert(`${viewer} has${viewers.has(viewer) ? '' : ' not'} watched this video.`);
    viewer = prompt('Who else do you want to follow up?', 'email');
}
```
`delete(value)` om een value uit een Set te verwijderen
```javascript
viewer = prompt('Who do you want to remove from the set of viewers?', 'email');
while (viewer) {
    alert(`${viewer} was ${viewers.delete(viewer) ? '' : 'not'} removed.`);
    viewer = prompt('Who else do you want to remove?', 'email');
}
```
`clear()` om de set in 1 keer te legen

`values()` retourneert een itereerbaar object met alle values

`entries()`retourneert een itereerbaar object met alle `[value, value]` pairs
- enkel voor compatibiliteit met de Map
```javadoc
message = 'All collections can hold a mix of values from different types...\n';
viewers.clear();
viewers.add(5);
viewers.add(['a', 'b', 'c']);
viewers.add(true);
viewers.add('aString');
viewers.add(x => x * 2);
viewers.add(new Map());

for (const viewer of viewers.values()) {
    message += `${(typeof viewer)}\n`;
}
alert(message);
```
`for .. of` lus kan rechtstreeks op de set gebruiken values
```javascript
message = 'All collections can hold a mix of values from different types...\n';
viewers.clear();
viewers.add(5);
viewers.add(['a', 'b', 'c']);
viewers.add(true);
viewers.add('aString');
viewers.add(x => x * 2);
viewers.add(new Map());

for (const viewer of viewers) {
    message += `${(typeof viewer)}\n`;
}
alert(message);
```
Op Set is ook de `forEach(callback)` gedefinieerd
```javascript
message = 'All strings in the set:\n’;
viewers.forEach((value) => message += typeof value === 'string' ? `${value}\n` : '');
alert(message);
```


## **Rest & spread syntax**

> ### **Read & spread syntax**
met de `spread` syntax kunnen we een iterable 'uitklappen' in afzonderlijke elementen. Deze gebruikt als:
- Argumenten bij functie aanroepen
- Elementen ve array bij de array literal notation
Volgende built-in iterable types kennen we (merk op dat Object niet in de lijst staat!)
- string
- map
- set
- array
Voorbeeld:
```javascript 
const numbers = [20, 30, 40, 50];

//in een functie aanroepen
Math.max(1, 20, 30,40, 50, 8);
Math.max(10, ...numbers , 8);

//in een array literal
const numbers2 = [-1, 5, 11, 20, 30,40, 50];
const numbers2 = [-1, 5, 11, ...numbers];

//spread syntax & array literals
const aString = 'Javascript';
console.log([...aString]);

const anArray = ['a', 'b', 'c'];
console.log([1, 2, ...anArray, 3, 4]);

const aMap = new Map([
    ['Belgium', 11589623],
    ['Burkina Faso', 20903275],
    ['Iceland', 341243],
]);
console.log([1, 2, ...aMap, 3, 4]);

const aSet = new Set(["tom.antjon@hogent.be", "
stefaan.decock@hogent.be"]);
console.log([1, 2, ...aSet, 3, 4]);

//spread syntax & functie aanroepen
const numbers = [-1, 5, 11, 3];

console.log(Math.max(...numbers));
console.log(Math.max(1, 10, ...numbers, 20, 2));
console.log(Math.max(...aMap.values()));

//arrays samenvoegen
const arr1 = ['Jan', 'Piet'];
const arr2 = ['Joris', 'Korneel'];
// maak een shallow copy die de inhoud van beide arrays bevat:
const arr12 = [...arr1, ...arr2];
console.log(arr12); // ["Jan", "Piet", "Joris", "Korneel"]
// voeg aan arr1 de elementen van arr2 toe
arr1.push(...arr2);
console.log(arr1); // ["Jan", "Piet", "Joris", "Korneel"]
```
Je kan **iterables, zoals maps, sets, ... omvormen tot arrays** om zo gebruik te kunnen maken van de krachtige arraymethodes.
1. Converteer de iterable naar een array via `[…yourMapOrSet]`
2. Maak gebruik van **array-methodes**
3. Converteer het resultaat terug naar een map/set via gebruik van de **constructor** waarbij je de **initiële waarden aanlevert via de array**

Voorbeeld: de keys van een map alfabetisch sorteren
```javascript
console.log(`Before sort:`);
console.log(population);
population = new Map([...population].sort(
    ([key1], [key2]) => {
        if (key1 < key2) return -1;
        if (key1 > key2) return 1;
        return 0;
    }));
console.log(`After sort:`);
console.log(population);
```
Voorbeeld 2: we willen onze map population aanpassen zodat enkel landen met meer dan 5000000 inwoners overblijven
```javascript
console.log(`Original map:`);
console.log(population);

population = new Map([...population].filter(
    ([country, population]) => population > 5000000));

console.log(`Map without big countries:`);
console.log(population);
```
Voorbeeld 3: we willen de inhoud van twee maps samenvoegen
```javascript
console.log('Landen in eerste map:');
console.log(population);
console.log('Landen in tweede map:');
console.log(population2);
const combinedPopulation = new Map([...population, ...population2]);
console.log('Alle landen samen in 1 map:');
console.log(combinedPopulation);
```
Via de **rest parameter** syntax kunnen we een onbepaald aantal argumenten aanleveren aan de **parameter van een functie**
- Dit moet de **laatste parameter** van de functie
- Die parameter is een **array waarin alle ‘overige’ argumenten worden verzameld**
```javascript
function showName(lastname, ...firstnames) {
    console.log(`De parameter firstnames bevat ${firstnames}`);
    const i = firstnames.reduce((initials, current) => initials + current[0], '');
    return `${i} ${lastname}`;
}

console.log(showName('Rowling', 'Joanne', 'Kathleen'));
console.log(showName('Rubens', 'Pieter', 'Paul'));
```
Het **rest** pattern laat ook toe het resterende deel van een array vast te nemen in een variabele tijdens **array destructuring**
- Je kan de rest operator enkel bij de laatste in de rij van variabelen
zetten
```javadoc
const [a, ...b] = ['Jan', 'Piet', 'Korneel', 'Steven', 'Maarten'];
console.log(a);
console.log(b);
```