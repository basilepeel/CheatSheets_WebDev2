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
begint      // hele grote gehele getallen
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
    - gehele getallen: [-(2^53 - 1), 2^53 -1]
- Integer literals
    - decimaal: 3, 10, 10000000, 10_000_000, …
    - prefix 0x voor hexadecimaal
    - prefix 0o voor octaal
    - prefix 0b voor binair
- Floating point literals [digits][.digits][(E|e)[(+|-)]digits]
    - 3.14, 2345.789, 0.33333333333333, 6.02e23
- Fasis operatoren: +, -, *, /, %
- Ffrondingsfouten
    - precisie tot op 17 cijfers na de komma
- Speciale waarden
    - **NaN** (Not a Number)
        - om iets dat niet kan voorgesteld worden als getal te beduiden
        - `isNan()` is een boolse functie die detecteerd of een waarde NaN is
    - **Infinity**
        - een berekening die een getal retourneert buiten het waardenbereik, retourneert Infinity of –Infinity
        - `isFinite()` is een boolse functie die detecteert of een waarde finite is
- **string - number conversie** functies
    - `parseInt(aString [, radix])`
        - converteert string naar geheel getal
            - leading spaces in de string worden
                genegeerd
            - eerste beduidende positie moet +,- of cijfer zijn
            - de radix is optioneel
    - `parseFloat(aString)`
        - converteert string naar floating point
    - beide doorlopen de string tot ze het eerste ongeldige karakter tegenkomen

> ### **Het datatype boolean** 
kan een logische waarde bevatten
 - **true** of **false** (case sensitive!)
    - alle waarden in JS hebben een bools equivalent, dit wordt
        gebruikt bij impliciete conversies

> ### **Het datatype string**
bevat een sequentie van 0 of meer Unicode karakters vervat in ‘ of
“
- opening en closing quote stijl moet dezelfde zijn
- gebruik \ voor escape sequences
    - \”, \’, \n, \ \
- + operator: concatenatie van 2 strings
- merk op: er is geen type char voor één enkel karakter voor te stellen

Conversie naar een string
```javascript
toString()
```
- **template literals** strings die variabelen & explressies bevatten
    - worden omsloten door backticks `
    - multiline strings (zonder \n) zijn mogelijk
    - expressies in de string evalueren: `${expressie}`

> ### **Het datatype undefinded**
kan enkel de speciale waarde undefinded bevatten
- staat voor een onbekende waarde
    - een variabele waaraan nog geen waarde is toegekend
        bevat de waarde undefined
    - een functie in JS retourneert steeds een waarde, indien
        er niet expliciet een waarde wordt geretourneerd dan
        wordt er impliciet undefined geretourneerd

> ### **Het datatype null**
kan enkel de waarde `null` bevatten
- staat voor een lege object pointer
- `typeof null` retourneert "object"
- evalueert `false` in een boolean expressie

> ### **Het datatype bigint**
om gehele getallen voor te stellen die buiten het bereik van number vallen
 - _< 2^53 -1 (zonder underscore)
 - _> 2^53 -1 (zonder underscore)

gebruik de `suffix n` voor begint literals

> ### **Het datatype object**
voorstelling van meer complexe datastructuren

> ### **Het datatype symbol**
om **unieke identifiers** voor te stellen
- kan gebruikt worden om bv. unieke namen te hebben voor properties in objecten

## **Bouwstenen: wrapper objects**

> ### **Wrapper object**
- is een object dat een primitief datatype ‘omsluit’
- bestaat voor elk primitief datatype behalve `null` en
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

- het wrapper object geeft ons de mogelijkheid properties en methodes te gebruiken
    - properties – eigenschappen
    - methodes – functies

> ### **Het wrapper object Number**
- JavaScript past impliciete conversie toe, het primitieve getal wordt automatisch omgezet naar een Number object als je een methode van Number oproept
- voorbeeldend van **methodes**
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
type-casting met String() roept in feite toString() aan maar één verschil is dat String() ook een null of undefined kan omzetten zonder fout te genereren:
```javascript
String();

const getal = 12;
console.log(String(getal)); //>> "12";
console.log(String(null)); //>> "null";
```

> ### **Het wrapper object BigInt**
maak een BigInt:
```javascript
BigInt(x);
```

## **Bouwstenen: Math & Date object**

> ### ** Het Math-object**
- een built-in object dat properties en methodes bevat voor wiskundige constanten en functies
- bevat enkel static properties en methods
    - gebruik steeds `Math.` om te refereren naar een property/methode
```javascript
Math.round(), Math.trunc() //afronden of afkappen
Math.max(), Math.min() //grootste/kleinste getal
Math.random() //pseudo random getal tussen 0 (incl) en 1(excl)
```
> ### **Het Date-object**
- houdt datums bij in milliseconden sinds 1/1/1970 UTC
- kan datums bijhouden 285.616 jaren ervoor en erna
- creatie datum : 
```javascript
const date = new Date(); //bevat de huidige datum/tijd
const date = new Date(1954,11,14,5,34,0,0) //jaar (4pos), maand (begint vanaf 0, dus waarde tussen 0 en 11), dag, uren, minuten, sec, msec)
```
- enkele methodes: 
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

 Herhalingen onderbreken
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