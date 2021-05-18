# Hoofdstuk 7: Asynchroon - callback - promise


## **Synchroon programmeren**
- Synchroon programmeren is een functie aanroepen, wachten op het antwoord van deze functie vooraleer je een volgende functie aanroept

Voorbeeld:
```javascript
function buildDeathStar(){
//do a lot of stuff, it takes time to build a death star 
const planet = {name: 'Alderaan', moon:false}; 
console.log(`Builded ${planet.name}`);
return planet;
}
function destroyPlanet(planet){
//do a lot of stuff, it takes time to build a death star
console.log(`Destroyed ${planet.name} `);
return planet.name === 'Alderaan'?'Aaaaaaargh, Alderaan destroyed\n':'Nothing seriously happened\n';
}
function aMillionVoicesCriedOut(cry){
   return cry.repeat(5);
}
let notAMoon = buildDeathStar();
let alderaan = destroyPlanet(notAMoon);                 // functie moet wachten tot buildDeathstar() klaar is
let disturbedForce = aMillionVoicesCriedOut(alderaan);  // functie moet wachten tot destroyPlanet() klaar is
```
Uitvoer:
```
 Builded Alderaan                   (functie buildDeathStar)
 Destroyed Alderaan                 (functie destroyPlanet)
 Aaaaaaargh, Alderaan destroyed     (functie aMillionVoicesCriedOut 5 keer)
 Aaaaaaargh, Alderaan destroyed
 Aaaaaaargh, Alderaan destroyed
 Aaaaaaargh, Alderaan destroyed
 Aaaaaaargh, Alderaan destroyed
```


## **Asynchroon programmeren**
- Bij asychroon programmeren wacht je niet tot een functie beëindigt is om een nieuwe functie te starten

Voorbeeld:
```javascript
function buildDeathStarAsync(){
//do a lot of stuff, it takes time to build a death star (10sec simulated by setTimeOut) setTimeout(function(){
const planet = {name:'Alderaan', moon:false }
console.log(`Builded ${planet.name}, it took 10 seconds`); 
//return, do not work with timeout
return planet;
}, 10000) }
function createACloneArmyAsync(){
   console.log('Cloned Army, go for war...');
}
function destroyPlanet(planet){
//do a lot of stuff, it takes time to destroy a death star (2sec)
   setTimeout(function(){
console.log(`Destroyed planet, it took 2 seconds`);
console.log(planet.name === 'Alderaan'?'Aaaaaaargh, Alderaan destroyed\n'.repeat(10):'Nothing seriously happened')
},2000); }
let notAMoon = buildDeathStarAsync();
createACloneArmyAsync();
destroyPlanet(notAMoon);
```
Uitvoer:
```
Cloned Army, go for war...
Destroyed planet, it took 2 seconds
> Uncaught TyperError: Cannot read property 'name' of undefined at asynchroon.js:15
Builded Alderaan, it took 10 seconds
```
Oplossing voor deze error -> [Callback Functies](#Callback-functies)

## **Callback functies**
>### **Callback**
- Callback functies bieden een oplossing voor dit probleem.
We willen namelijk niet wachten op het resultaat van een functie om een andere
functies te starten, die geen dependency hebben met de eerste functie.
- In plaats van het result te retourneren en dit result te gebruiken door een
andere functie, gaan we de andere functie (callback functie) als parameter
meegeven aan de lang durende functie.
```javascript
function longAsyncOperation(callbackParameter) {
     // loooong operation
     // return calculation;
     callbackParameter(calculation);
}
longAsyncOperation(doSomethingWithResult);
doOtherStuffThatDoesntNeedResult();
```

Voorbeeld:
```javascript
function buildDeathStarAsync(callbackParameter){
   //do a lot of stuff, it takes time to build a death star (10sec)
   setTimeout(function(){
      const planet = {name: 'Alderaan', moon:false}
      console.log(`Builded ${planet.name}, it took 10 seconds`);
     callbackParameter(planet)
},10000); }
function createACloneArmyAsync(){
   console.log('Cloned Army, go for war...');
}
function destroyPlanet(planet){
   //do a lot of stuff, it takes time to destroy a death star (2sec)
   setTimeout(function(){
      console.log(`Destroyed ${planet.name}, it took 2 seconds`);
      console.log(planet.name === 'Alderaan'?'Aaaaaaargh, Alderaan destroyed\n'.repeat(10):'Nothing seriously happened')
},2000); }
buildDeathStarAsync(destroyPlanet);
createACloneArmyAsync();
```
Uitvoer:
```
 Builded Alderaan                   (functie buildDeathStar)
 Destroyed Alderaan                 (functie destroyPlanet)
 Aaaaaaargh, Alderaan destroyed     (functie aMillionVoicesCriedOut 5 keer)
 Aaaaaaargh, Alderaan destroyed
 Aaaaaaargh, Alderaan destroyed
 Aaaaaaargh, Alderaan destroyed
 Aaaaaaargh, Alderaan destroyed
```
- Callback functies worden vaak gekoppeld aan eventhandlers (click, load, keyup, ...). Dit is vrij logisch omdat je niet weet wanneer en hoe lang het event zal plaatsvinden. De callback zal dan ook pas worden uitgevoerd na het plaatsvinden van het event.
- Een callback functie kan op verschillende manieren
gedeclareerd worden:
    - function nameFunction(par){//statements}
    - const nameFunction = function(par){//statements}
    - Anonymous function: function(par){//statements}
    - Arrow notation: (par)=>{//statements}
- We koppelen de callback function aan het event met:
    - de addEventlistener method van het element object
    - toewijzen van de functie aan het event van het element object
```javascript
elText = document.getElementById("textButtons");
//alle button elementen ophalen
const buttons = document.getElementsByTagName('button'); [...buttons].map((item)=>{
//callback koppelen aan click event dmv onclick
   switch (btn.id){
        case 'b1':
            btn.onclick = clickMe;
            break;
        case 'b2':
            btn.onclick = clickMeToo; 
            break;
        case 'b3':
            btn.onclick = function(event){showText(event);};
            break;
        case 'b4':
            btn.onclick = event=>{showText(event);};
            break;
        default: elText.innerHTML = "Something went wrong";
    } 
})
```
>### **Callback hell**
- Callbacks werken prima zolang het kleine, geïsoleerde functies zijn.
Van zodra de callback zelf callbacks heeft en deze dan ook nog eens een callback, enzovoort enzoverder, dan krijgen we een zogenaamde “Callback Hell”, en dat is allesbehalve overzichtelijk en leidt bijna zeker tot fouten...
>### **Callback problemen**
- Een ander probleem doet zich voor als:
    - twee parallelle asynchrone functies gelijktijdig worden opgestart.
    - Wanneer kan een derde functie gestart worden nadat beiden parallel opgestarte functies afgehandeld zijn?
- Om deze problemen, de callback hell en het tijdstip om een functie op te starten nadat asynchrone functies gelijktijdig zijn opgestart, op te lossen gaan we gebruik maken van promises (ook futures genoemd).

## **Promises**
>### **Promise**
- Het idee achter promises is het resultaat van een asynchrone operatie bij te houden in een variabele en deze te gaan gebruiken waar en wanneer je wilt.
```javascript
 let deathStarPromise = buildADeathStarAsync();
 let army = buildCloneArmyAsync();
 ```
- De asynchrone operatie buildADeatStarAsync() wordt toegekend aan deathStarPromise.
- Deze zal natuurlijk niet onmiddellijk het resultaat bevatten, maar houdt de toestand van de asynchrone operatie bij, het is een “placeholder”: dit is de promise
- De volgende functie kan gestart worden, zonder rekening te houden met de toestand van de eerste functie.
- Je kan reageren met het toekomstige resultaat van de eerste asynchrone functie door gebruik te maken van een `then` method, die een callback bevat als parameter, van de promise. Deze callback wordt uitgevoerd van zodra de buildADeatStarAsync succesvol is afgehandeld
```javascript
let deathStarPromise = buildADeathStarAsync();
let army = buildCloneArmyAsync();
deathStarPromise.then(notAMoon => notAMoon.destroyPlanet());
let luke = meetYourDad();
deathStarPromise.catch(error => console.log(error));
```
- Indien de buildADeathStarAsync niet succesvol is afgehandeld kan men via de catch method van de promise dit ook gaan afhandelen via een callback.
>### **Verschil tussen promise en callback**
- Op het eerste zicht is er weinig verschil met callback. Je gaat pas een functie aanroepen (callback) van zodra de asynchrone functie is afgehandeld (then method van de promise).
- Het verschil is dat bij een promise je steeds controle hebt op het moment dat je het resultaat van de asynchrone functie kan gebruiken (then method). De code wordt ook veel meer leesbaar. De code van het voorbeeld “callback hell” wordt plots veel leesbaarder.
>### **Toestanden van een promise**
- Pending
    - Het resultaat van de promise is nog niet bepaald aangezien de asynchrone operatie die verantwoordelijk is voor het resultaat, nog niet afgerond is.
    - Wanneer een promise ‘pending’ is, kan het overgaan naar de toestand ‘fulfilled’ of ‘rejected’.
    - Eens een promise ‘fulfilled’ of ‘rejected’ is, kan het niet meer overgaan naar een andere status.
    - Zijn value of de reden waarom de operatie faalde, zal niet meer veranderen.
- Fullfilled
    - De asynchrone operatie is afgerond en de promise heeft een waarde.
- Rejected
    - De asynchrone operatie is niet goed afgerond waardoor de promise niet kan gerealiseerd worden. In deze status heeft de promise een reden die aangeeft waarom de operatie faalde.
>### **Algemene structuur van een promise**
```javascript
const p = new Promise((resolve, reject) => {
    // Voer een asynchrone taak uit en vervolgens ...
    if(/* De asynchrone taak is gelukt */) { 
        resolve(resolveWaarde);
    } else {
        reject(rejectWaarde);
    }
});
p.then((resolveWaarde) => {
    /* Doe iets met het resultaat.
    Hier komt de actie die moet uitgevoerd worden als het resultaat beschikbaar is. */
}).catch((rejectWaarde) => {
    /* Error :(
    Hier komt de actie die moet uitgevoerd als de taak faalde */
})
```

>### **Beperkingen van promise
- Promises maken het werken met asynchrone code een stuk aangenamer, perfect zijn ze natuurlijk niet.
- Een eerste probleem is dat een promise altijd start, en altijd afgerond wordt, dus ook als er nooit iemand .then() aanroept, of als het resultaat je niet langer interesseert kan je een promise niet 'cancel'-en
- Eenmaal een promise afgerond is, kan je ze ook niet makkelijk opnieuw uitvoeren, de promise zelf bevat enkel het resultaat.
- Om deze problemen aan te pakken hebben we een andere constructie nodig: OBSERVABLES (Webapplicaties IV)