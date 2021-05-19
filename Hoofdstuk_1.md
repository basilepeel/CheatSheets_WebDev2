# **Hoofdstuk 1: JavaScript**




## **Aan de slag met JS!**

Via het src-attribuut van het script element in het HTML bestand, verwijs je naar het apparte JavaScript bestand.
``` javascript
<script src = ".../js/hello.js"> </script>
```




## **Code structuur**

> ### **Statements**
Toont de uitvoer in de console.
```javascript
console.log()
```
Toont de uitvoer in een simpel dialoogvenster.
```javascript
alert()
```

> ### **Comments**
Commment over één lijn.
```javascript
//comment
```
Comment over meedere lijnen.
```javascript
/*dit is
  een
  comment*/
```

> ### **JS strict mode**
JavaScript runnen in 'strict mode'. 
```javascript
"use strict"
```




## **Bouwstenen: variabelen**

> ### **Variabelen**
Gebruik het keyword `let` om een variabele te declareren.
 - De naam van een variabele mag enkel **letters, cijfers en de symbolen $ en _** bevatten en de naam mag niet starten met een cijfer.
 - Gebruik camelCasing.
 - **Hoofdlettergevoelig!**
```javascript
let
```
Via de assignment operator `=` kan je een waarde toekennen aan een variabele.
```javascript
let message;
message = 'Hello';
```
Je kan declaratie en assignment (~initialisering) combineren.
```javascript
let message = 'Hello";
```
> ### **Constanten**
Variabelen die je moet initialiseren bij declaratie en die nadien niet meer van waarde kunnen veranderen.
```javascript
const
```




## **Bouwstenen: basis datatypes**

> ### **Datatypes**
JS kent 8 basis datatypes:
```javascript
number      // getallen
bigint      // hele grote gehele getallen
string      // tekst bestaande uit 0 of meerdere karakters
boolean     // logsiche waarden: true / false
null        // voor ongekende waarden
undefined   // voor niet toegekende waarden
object      // voor meer complexere datastructuren
symbol      // voor unieke identiefiers
```
Achterhaal het type van de variabele x in string-vorm:
```javascript
typeof(x);
typeof x;
```

> ### **Het datatype *Number***
- Gehele én floating point getallen
- 64-bit double precision floating point representation
    - Gehele getallen: [-(2^53 - 1), 2^53 -1]
- Integer literals
    - decimaal: 3, 10, 10000000, 10_000_000, …
    - prefix 0x voor hexadecimaal
    - prefix 0o voor octaal
    - prefix 0b voor binair
- Floating point literals [digits][.digits][(E|e)[(+|-)]digits]
    - 3.14, 2345.789, 0.33333333333333, 6.02e23
- Basis operatoren: +, -, *, /, %
- Afrondingsfouten
    - Precisie tot op 17 cijfers na de komma
- Speciale waarden
    - **NaN** (Not a Number)
        - Om iets dat niet kan voorgesteld worden als getal te beduiden
        - `isNan()` is een boolse functie die detecteerd of een waarde NaN is
    - **Infinity**
        - Een berekening die een getal retourneert buiten het waardenbereik, retourneert Infinity of –Infinity
        - `isFinite()` is een boolse functie die detecteert of een waarde finite is
- **string - number conversie** functies
    - `parseInt(aString [, radix])`
        - Converteert string naar geheel getal
            - Leading spaces in de string worden
                genegeerd
            - Eerste beduidende positie moet +,- of cijfer zijn
            - De radix is optioneel
    - `parseFloat(aString)`
        - Converteert string naar floating point
    - Beide doorlopen de string tot ze het eerste ongeldige karakter tegenkomen

> ### **Het datatype boolean** 
Kan een logische waarde bevatten
 - **true** of **false** (case sensitive!)
    - Alle waarden in JS hebben een bools equivalent, dit wordt
        gebruikt bij impliciete conversies

> ### **Het datatype string**
Bevat een sequentie van 0 of meer Unicode karakters vervat in ‘ of
“
- Opening en closing quote stijl moet dezelfde zijn
- Gebruik \ voor escape sequences
    - \”, \’, \n, \ \
- + operator: concatenatie van 2 strings
- MERK OP: er is geen type char voor één enkel karakter voor te stellen

Conversie naar een string
```javascript
toString()
```
- **template literals** strings die variabelen & explressies bevatten
    - Worden omsloten door backticks `
    - Mltiline strings (zonder \n) zijn mogelijk
    - Expressies in de string evalueren: `${expressie}`

> ### **Het datatype undefinded**
Kan enkel de speciale waarde undefinded bevatten
- Staat voor een onbekende waarde
    - Een variabele waaraan nog geen waarde is toegekend
        bevat de waarde undefined
    - Een functie in JS retourneert steeds een waarde, indien
        er niet expliciet een waarde wordt geretourneerd dan
        wordt er impliciet undefined geretourneerd

> ### **Het datatype null**
Kan enkel de waarde `null` bevatten
- Staat voor een lege object pointer
- `typeof null` retourneert "object"
- Evalueert `false` in een boolean expressie

> ### **Het datatype bigint**
Om gehele getallen voor te stellen die buiten het bereik van number vallen
 - _< 2^53 -1 (zonder underscore)
 - _> 2^53 -1 (zonder underscore)

Gebruik de `suffix n` voor begint literals

> ### **Het datatype object**
Voorstelling van meer complexe datastructuren

> ### **Het datatype symbol**
Om **unieke identifiers** voor te stellen
- Kan gebruikt worden om bv. unieke namen te hebben voor properties in objecten




## **Bouwstenen: wrapper objects**

> ### **Wrapper object**
- Is een object dat een primitief datatype ‘omsluit’
- Bestaat voor elk primitief datatype behalve `null` en
    `undefined`
    - String
    - Number
    - BigInt
    - Boolean
    - Symbol

Achterhaal de primitieve waarde voorgesteld door het wrapper objects
```javascript
valueOf()
```

- Het wrapper object geeft ons de mogelijkheid properties en methodes te gebruiken
    - Properties – eigenschappen
    - Methodes – functies

> ### **Het wrapper object Number**
- JavaScript past impliciete conversie toe, het primitieve getal wordt automatisch omgezet naar een Number object als je een methode van Number oproept
- Voorbeeldend van **methodes**
```javascript
toFixed(x)  // afronden tot x cijfers na de komma
toString(x) // string representatie, x is de radix, default 10
```
- voorbeelden van **properties**
```javascript
Number.MIN_VALUE    // constante voor kleinste getal (5e-324)
Number.MAX_VALUE    // grootste getal (-1.7976931348623157e+308)
Number.NaN          // idem als NaN, maar Property van Number
```

> ### **Het wrapper object Boolean**
Om x naar een Boolean te casten:
```javascript
Boolean(x)
```

> ### **Het wrapper object String**
Type-casting met String() roept in feite toString() aan maar één verschil is dat String() ook een null of undefined kan omzetten zonder fout te genereren:
```javascript
String();

const getal = 12;
console.log(String(getal)); //>> "12";
console.log(String(null)); //>> "null";
```

> ### **Het wrapper object BigInt**
Maak een BigInt:
```javascript
BigInt(x);
```



## **Bouwstenen: Math & Date object**

> ### ** Het Math-object**
- Een built-in object dat properties en methodes bevat voor wiskundige constanten en functies
- Bevat enkel static properties en methods
    - gebruik steeds `Math.` om te refereren naar een property/methode
```javascript
Math.round(), Math.trunc() //afronden of afkappen
Math.max(), Math.min() //grootste/kleinste getal
Math.random() //pseudo random getal tussen 0 (incl) en 1(excl)
```
> ### **Het Date-object**
- Houdt datums bij in milliseconden sinds 1/1/1970 UTC
- Kan datums bijhouden 285.616 jaren ervoor en erna
- Creatie datum : 
```javascript
const date = new Date(); //bevat de huidige datum/tijd
const date = new Date(1954,11,14,5,34,0,0) //jaar (4pos), maand (begint vanaf 0, dus waarde tussen 0 en 11), dag, uren, minuten, sec, msec)
```
- Enkele methodes: 
```javascript
getDate     //bevat de huidige datum/tijdreturnt dag van de maand
getMonth()  //start vanaf 0!!!!!!! (0-11)
getFullYear()
getHours() (0-23), getMinutes() (0-59), getSeconds() (0-59)
getDay      //dag in de week (0-6)
```



## **Controlestructuren**

> ### **Controlestructuren**
 - `blocks`
 - `if`
 - `while` / `do..while`
 - `for`
 - `break` / `continue`
 - https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Statements
 - code blokken worden afgebakend met accolades: 
 ```javascript
 {
     //code
 }
 ```
 Selectie met `if(..){..} else{..}` op basis van voorwaarden
 ```javascript
 if (/*voorwaarde*/){
     //deze code wordt gerunned als aan de voorwaarde wordt voldaan
 }

 if (/*voorwaarde*/){
     //deze code wordt gerunned als aan de voorwaarde wordt voldaan
 } else {
     //deze code wordt gerunned als er niet aan de voorwaarde wordt voldaan
 }

 if (/*voorwaarde 1*/){
     //deze code wordt gerunned als aan de voorwaarde wordt voldaan
 } else  if (/*voorwaarde 2*/){
     // deze code wordt gerunned als er aan voorwaarde 2 wordt voldaan
 } else {
     //deze code wordt gerunned als er niet aan de voorwaarde 2 wordt voldaan
 }
 ```
Selectie met `switch().. case.. ` om een selectie te maken uit meerdere blokken code
```javascript 
function doSwitch() {
    const d = new Date();
    const theDay = d.getDay();
    switch (theDay) {
        case 5:
            console.log('Finally Friday');
            break;
        case 6:
            console.log('Super Saturday');
            break;
        case 0:
            console.log('Sleepy Sunday');
            break;
        default:
            console.log("I'm really looking forward to this weekend!");
    }
}
```
Iteratie met `while(..) do{..}` en `do{..} while(..)`
```javascript
function doWhile() {
    let scoops = 10;
    while (scoops > 0) {
        console.log('More icecream!');
        scoops--;
    }
    console.log("life without ice cream isn't the same");

    do {
        console.log('More icecream!');
        scoops++;
        } while (scoops < 10);
            console.log(':)');
}
```
Output:
```More icecream!
 More icecream!
 More icecream!
 More icecream!
 More icecream!
 More icecream!
 More icecream!
 More icecream!
 life without ice cream isn't the same
  ```
  Iteratie met `for(..) {..}`
```javascript
function doFor() {
    for (let berries = 5; berries > 0; berries--) {
        console.log('Eating a berry');
    }
    console.log('No more berries left');
}
```
Output:
```Eating a berry
 Eating a berry
 Eating a berry
 Eating a berry
 Eating a berry
 No more berries left
 ```

 Herhalingen **onderbreken**
- `break;` de volledige herhaling wordt gestopt - voorwaarde wordt niet getest
```javascript
function doJumpingOut() {
    //break : jumping out
    let k = 0;
    while (true) {
        k++;
        if (k > 5)
            break;
    console.log(`Waarde voor k : ${k}`);
}
```
Output:
```
 Waarde voor k : 1
 Waarde voor k : 2
 Waarde voor k : 3
 Waarde voor k : 4
 Waarde voor k : 5
```

- `continue;` stopt de huidige herhaling, test de voorwaarde en voert eventueel de        herhaling verder uit.
```javascript
function doJumpingOut() {
    //skipping with continue
    let l = -3
    while (l < 3) {
        l++;
        if (l < 0)
            continue;
        console.log(`Waarde voor l : ${l}`);
    }
}
```
Output:
```
Waarde voor 1: 0
Waarde voor 1: 1
Waarde voor 1: 2
Waarde voor 1: 3
```



## **Operatoren**

> ### **Operatoren**
Berekeningsoperatoren:
- +, -, *, /, %, ++, --, unary -, unary +

Toewijzingsoperatoren:
- =, *=, /=, %=, +=, -=, <<=, >>=, >>>=, &=, ^=, |=

Vergelijkingsoperatoren
- ==, !=, ===, !==, >, >=, <, <=

Logische operatoren:
- && = AND, || = OF, ! = NOT

String operatoren: 
- += en + 

**Er zijn 2 operatoren van gelijkheid!**
- == (en !=)
    - er gebeurt impliciete type conversie, m.a.w. het type wordt niet gecontroleerd
- === (en !==)
    - geen impliciete type conversie
        - best practice: gebruik ===




## **JavaScript debuggen**

> ### **JavaScript debuggen**
- Dev tools in browser -> Sources tab
- Breakpoints 




## **Functies**

> ### **Functies**
Defenitie:
```javascript
function functionname(par1,par2,...,parX) {
    statements
};
```
Uitvoeren:
```javascript
functionname(arg1, arg2,….argx);
```
Gebruik van bestaande functies:
- **Alert box** (pop-up met een "Ok" knop): 
```javascript
alert("sometext")
```
- **Confirm box** (pop-up met een "Ok" -en een "Annuleren" knop)
```javascript
confirm("sometext")
```
- **Prompt box** (pop-up met een "Ok" -en "Annuleren" knop, en met een tekstveld)
    - Als geen defaultValue voorzien wordt null geretourneerd
```javascript
prompt("sometext","defaultValue")
```
Voorbeeld:
```javascript
function doAlert () {
    alert(‘I am an alert box!’);
}

function doConfirm () {
    const antwoord = confirm(‘Press a button’);
    if (antwoord === true)
        alert(‘You pressed OK!’);
    else
        alert(‘You pressed Cancel!’);
}

function doPrompt () {
    const name = prompt(‘Please enter your name’, ‘Harry Potter’);
    if (name !== null && name !== "") {
        alert(`Hello ${name}! How are you today?`);
}
```

> ### **Hoisting**
- Functie en variabele declaraties worden eerst gecompileerd alvorens de browser de script code uitvoert
- Voorbeeld: 
```javascript
sayHi('Bob'); //alert: Hi, my name is Bob

function sayHi(name) {
alert(`Hi, my name is ${name}`);
}

sayHi('Bob'); //alert: Hi, my name is Bob
```


## **Arrays**

> ## **Arrays**
- Een array is een geordende verzameling van elementen. De elementen mogen van een verschillend type zijn
- Elk element heeft een genummerde positie in de array, **index** genaamd. (**eerste element heeft index 0**)
- Arrays zijn dynamisch
- Notatie:
```javascript
[]
```
**Declaratie** van een lege array op twee manieren:
```javascript
let pizzas = [];

let pizzas = new Array();
```
Waarden van mogelijks verschillende types in een array **plaatsen**
```javascript
pizzas[0] = 'Margherita';
pizzas[1] = 'Mushroom';
pizzas[2] = 'Spinach & Rocket';
```
Waarden uit een array **wijzigen**
```javascript
pizzas[0] = 'Ham & Pineapple';
```
**Aanmaken** van array via **array literal**
```javascript
let pizzas =
['Margherita', 'Mushroom', 'Spinach & Rocket', 'Pineapple & Sweetcorn'];
```
Voorbeeld met elementen van verschillend type
```javascript
let mixedArray = [null, 1, 'two', true, undefined ];
```
**Opvragen van 1 element** adhv de index
    - `index` start vanaf 0, gebruik `[]`
    - `length`: aantal elementen in een array
```javascript
const pizzas =
    ['Margherita', 'Mushroom', 'Spinach & Rocket', 'Pineapple & Sweetcorn'];

console.log(pizzas[2]); //Spinach & Rocket
console.log(`eerste pizza ${pizzas[0]}`); //Margherita
console.log(`laatste pizza ${pizzas[pizzas.length-1]}`); //Pineapple & Sweetcorn
```
**Volledige array printen**
```javascript
console.log(pizzas); //Array(4) ['Margherita', 'Mushroom', 'Spinach & Rocket', 'Pineapple & Sweetcorn'];
```
**Verwijderen** van een element in de array
    - Verwijdert de waarde op deze positie, maar de ruimte bestaat nog steeds en bevat nu de waarde `undefined`.
```javascript
const pizzas =
    ['Margherita', 'Mushroom', 'Spinach & Rocket', 'Pineapple & Sweetcorn'];

delete pizzas[2];

console.log(pizzas); // ['Margherita', 'Mushroom', undefined, 'Pineapple & Sweetcorn']; 
```
**Overlopen** van een array
- `for-loop`
```javascript
const pizzas =
    ['Margherita', 'Mushroom', 'Spinach & Rocket', 'Pineapple & Sweetcorn'];

for (let i = 0; i < pizzas.length; i++) {
    console.log(pizzas[i]);
}
```
- `for-of loop`
```javascript
const pizzas =
    ['Margherita', 'Mushroom', 'Spinach & Rocket', 'Pineapple & Sweetcorn'];
for (let value of pizzas) {
    console.log(value);
}
```
- Merk op:
```javascript
const pizzas =
    ['Margherita', 'Mushroom', 'Spinach & Rocket', 'Pineapple & Sweetcorn'];
pizzas[30] = 'Vegetarian';
console.log(pizzas.length); // 31
console.log(pizzas[20]); //undefined
```
`pop` **vewrijdert het laatste element** uit de array en retourneert dit element
```javascript
pizzas.pop(); // << 'Spinach & Rocket'
```
`push` **voegt één of meerdere waarden toe aan het einde** van de array en retourneert de nieuwe lengte van de array.
```javascript
pizzas.push('Pepperoni'); // << 3
```
`shift` **verwijdert de eerste waarde** in array en returnt deze
```javadoc
pizzas.shift(); //<< 'Margherita'
```
`unshift` **voegt één of meerdere waarden toe aan het begin** van het array en **returnt de nieuwe lengte** van de array.
```javascript
pizzas.unshift('Chicken & Bacon'); //<< 3
```
**Zoeken** of een waarde voorkomt in een array met `indexOf`
- Retourneert index van eerste voorkomen of -1 als waarde niet voorkomt.
```javascript
const pizzas = ['Margherita', 'Mushroom', 'Spinach & Rocket];
pizzas.indexOf('Spicy Beef'); //<< -1
pizzas.indexOf('Margherita'); //<< 0
```
Andere Array methodes: 
- `concat()`: Voegt 2 arrays samen
- `reverse()`: Keert de volgorde van de array elementen om
- `slice(start_index, upto_index)`: Returnt een nieuw array als een stuk van de oorspronkelijke array met als argumenten de begin- en een eindpositie.
- `splice(start_index, numberofitemsToRemove, waarde1,…, waardex)`: Verwijdert numberofItemsToRemove waarden uit de array startend op positie start_index en voegt dan de nieuwe waarden toe waarde1,… waardex
- `sort()`: Sorteert de elementen in de array
- `indexOf(searchElement[, fromIndex])`: De index van het eerste voorkomen van het element vanaf fromIndex
- `lastIndexOf(searchElement[, fromIndex])`: Idem indexOf maar begint achteraan
- `join()`: Converteert alle elementen van een array tot 1 lange string 

**Destructuring**
- Is een manier om **meerdere waarden te extraheren** uit een array en **toe te kennen aan variabelen**
```javascript
//Variabele declaraties
//ophalen van eerste en tweede item uit een array
pizzas = ['Margherita', 'Mushroom', 'Spinach & Rocket', 'Chicken & Bacon’];
const [eerstePizza, tweedePizza] = pizzas;
console.log(eerstePizza); // Margherita
console.log(tweedePizza); // Mushroom

//ophalen van derde item uit een array
const [, , derdePizza] = pizzas;
console.log(derdePizza); //Spinach & Rocket

//Destructuring Assignment
pizzas = ['Margherita', 'Mushroom', 'Spinach & Rocket'];
let pizza1, pizza2;
[pizza1, pizza2] = pizzas;
console.log(pizza1); // Margherita
console.log(pizza2); // Mushroom

//default values
pizzas = ['Margherita'];
[pizza1, pizza2 = 'Mushrooms' ] = pizzas;
console.log(pizza1); // Margherita
console.log(pizza2); // Mushrooms

// Swapping variables in ECMAScript 5
let a = 1,b = 2, tmp;
tmp = a;
a = b;
b = tmp;
console.log(a); // 2
console.log(b); // 1

// Swapping variables in ECMAScript 6
a = 1;
b = 2;
[ a, b ] = [ b, a ];
console.log(a); // 2
console.log(b); // 1
```
Meerdimensionele arrays
- een array van een array = **2 dimensionale array**
- een array van een array van een array = **3 dimensionale array**
- Voorbeeld : een array die de kaarten van 2 poker hands bevat
```javascript
const hands = [];
hands[0] = [5,'A',3,'J',3];
hands[1] = [7,'K',3,'J',3];
console.log ('2de kaart, 2de hand : ' + hands[1][1]);
```
- Voorbeeld 2-dimentionale array
    - Een schaakbord, voorgesteld als een 2 dimensionele array van
        strings. De eerste zet verplaatst ‘p’ van positie (6,4) naar (4,4). 6,4
        wordt op blanco geplaatst
```javascript
function doArray() {
const board = [
['R', 'N', 'B', 'Q', 'K', 'B', 'N', 'R'],
['P', 'P', 'P', 'P', 'P', 'P', 'P', 'P'],
[' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
[' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
[' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
[' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '],
['p', 'p', 'p', 'p', 'p', 'p', 'p', 'p'],
['r', 'n', 'b', 'q', 'k', 'b', 'n', 'r']];

console.log(board.join('\n') + '\n\n');
board[4][4] = board[6][4]; // Move King's Pawn forward 2
board[6][4] = ' ';
console.log(board.join('\n'));
}
```
