# **Hoofdstuk 3: Object Oriented Programming in JavaScript**


## **Class declarations**

> ### **Class declaration**
```javascript
class ClassName {
    
    constructor (…) {…}
    
    method1(…) {…}
    
    get someProp() {…}
    set someProp(value) {…}
    
    static staticMethod(…) {…}
}
```
Voorbeeld: `BlogEntry`, een klasse voor een blog entry
```javascript
class BlogEntry {
    constructor(body, date) {
        this.body = body;
        this.date = date;
    }
}
```

## **Class declarations:** new & constructors

> ### **Class declaration**
`new`: instanties maken van een klasse
```javascript
const aBlogEntry = new BlogEntry('Managed to get a headache toiling over the new cube. Gotta nap.',
new Date(2008, 7, 16)
);
```
De **constructor**
- Gebruik keyword `constructor` met eventuele parameters
- Gebruik `this` in de constructor body om properties te definiëren en eventueel te initialiseren

**Default constructor**
- Indien in een klasse niet expliciet een constructor wordt gedefinieerd dan heeft de klasse impliciet een parameterloze constructor
```javascript
class NoCtorClass {}
const myObject = new NoCtorClass();
```

Er is **geen constructor overloading mogelijk**
- maximum 1 `const` definiëren



## **Class declarations:** properties: getters & setters

> ### **Class declaration**
Conventie i.v.m. private properties: `_`
<br>

**Getter**
- keyword `get`
- geen parameters
```javascript
class BlogEntry {
    constructor(body) {
        this._body = body;
        this._date = new Date();
    }
    get body() { return this._body; }
    get date() { return this._date; }
}

const aBlogEntry = new BlogEntry('...');
console.log(`body: ${aBlogEntry.body}`);
console.log(`date: ${aBlogEntry.date}`);

```
**Setters**
- keyword `set`
- exact 1 parameter	
```javascript
class BlogEntry {
    constructor(body) {
        this.body = body;
        this._date = new Date();
    }
    get body() { return this._body; }
    set body(value) { this._body = value; }
    get date() { return this._date; }
}

const aBlogEntry = new BlogEntry('...');
aBlogEntry.body = 'This is the way to go :)';
console.log(`body: ${aBlogEntry.body}`);
console.log(`date: ${aBlogEntry.date}`);

```
Voorbeeld: `class BlogEntry` samengevat:
```javascript
class BlogEntry {
    constructor(body) {
        this.body = body;
        this._date = new Date();
    }
    get body() { return this._body; }
    set body(value) { this._body = value; }
    get date() { return this._date; }
}

const aBlogEntry = new BlogEntry('...');
aBlogEntry.body = 'This is the way to go :)';
console.log(`body: ${aBlogEntry.body}`);
console.log(`date: ${aBlogEntry.date}`);
```
**Getter vs. parameterloze methode**
- De get-syntax laat toe om aan een berekende waarde van een object te geraken zonder een methode aan te roepen
```javascript
class BlogEntry {
    constructor(body) {
        this.body = body;
        this._date = new Date();
    }
    get nrOfWords() {
        return this._body.split(' ').length;
    }
    getNrOfWords() {
        return this._body.split(' ').length;
    }
// rest omitted for brevity
}

const myBlogEntry = new BlogEntry('Een twee drie vier vijf');
console.log(myBlogEntry.nrOfWords);
console.log(myBlogEntry.getNrOfWords());
```
Opmerkingen:
- Ook in object literals (cf. H01) kan je werken met get/set methodes
```javascript
const myAvatar = {
    _name: 'Bob',
    get name() {return _name;},
    set name(value) {_name = value;}
};
myAvatar.name = 'Ann';
```
- In een class kan je een property niet definiëren als key/value pair zoals in object literals, enkel methodes, getters en setters…
- **Voordelen** `get`/`set`
    - **Encapsulatie** van het gedrag die hoort bij manipulatie van een property
        - bv. validatie
    - **Verbergen** van de interne representatie van een property
        - De representatie naar buiten toe kan anders zijn
    - Zorgen dat de **interface van je klasse geïsoleerd** is tegen veranderingen
        - De implementatie van de klasse kan wijzigen zonder dat de client code aangepast hoeft te worden
    - Je kan **debuggen** op veranderingen in de property


## **Class declarations:** methodes

> ### **Class declaration**
Methodes
- **Functie-properties**
- Naam, paramaters, return waarde zie H02: Objects and functions
- Voorbeeld: `class BlogEntry` uitgebreid met methode
```javascript
class BlogEntry {
    constructor(body) {
        this.body = body;
        this._date = new Date();
}   
get body() { return this._body; }
set body(value) { this._body = value; }
get date() { return this._date; }
contains(searchText) {
    return searchText
    ? this._body.toUpperCase().includes(searchText.toUpperCase())
    : false;
    }
}

let searchText = '';
do {
    searchText = prompt('Enter search term (leave blank to stop searching)');
    if (searchText)
        alert(
            `aBlogEntry ${aBlogEntry.contains(searchText) ? 'contains' : 'does not contain'
        } ${searchText}.`
    );
} while (searchText);
```

Static methods
    - Worden aangeroepen zonder de klasse te instantiëren
        - Kunnen **niet** worden aangeroepen via instanties van de klasse 
    - Enkele voorgedefinieerde objecten bevatten heel wat interessante static methods
        - Vb. Math bevat enkel static properties/methods


## **Class declarations:** overerving

> ### **Class declaration**
**Overerving**
- `extends` om een subklasse ve klasse te maken
    - Subklasse heeft maar 1 superklasse (single inheritance)
- Mogelijk om methodes uit de superklasse te **overriden**
    - `super.method()` om te referen naar een methode vd superplasse
- `super()` (in de constructor) gebruik maken van de constructor vd superklasse 
    - aanroep moet gebeuren + vóór `this`
    - indien geen constructor in subklasse gemaakt, krijgt subklasse een **default counstructor**:
```javascript
class B extends A {
    constructor(...args) { super(...args); }
}
```
Voorbeeld: `TaggedBlogEntry extends BlogEntry`:
```javascript
class TaggedBlogEntry extends BlogEntry {
    constructor(body, tags) {
        super(body);
        this._tags = tags;
    }
    get tags() {
        return this._tags;
    }
    addTag(tag) {
        this._tags.push(tag);
    }
    removeTag(tag) {
        const index = this._tags.indexOf(tag);
        if (index !== -1) {
            this._tags.splice(index, 1);
        }
    }
    contains(searchText) {
        return super.contains(searchText) || this.tags.includes(searchText);
    }
}
```
Voorbeeld: gebruik van de subklasse:
```javascript
const aTaggedBlogEntry = new TaggedBlogEntry( 'Today I want to write about my chickens.', ['pets', 'animals']);

console.log(aTaggedBlogEntry.body); // Today I want to write about my chickens
aTaggedBlogEntry.addTag('rooster');
console.log(aTaggedBlogEntry.tags); // (3) ["pets", animals", "rooster"]
console.log(aTaggedBlogEntry.contains('pets')); // true
console.log(aTaggedBlogEntry.contains('write')); // true
console.log(aTaggedBlogEntry.contains('cube')); // false
aTaggedBlogEntry.removeTag('animals');
console.log(aTaggedBlogEntry.tags); // (2) ["pets", "rooster"]
```
Opmerkingen:
- class declarations worden **niet gehoist**
    - Klasse declareren voor gebruik
    - Herinner: functional declarations worden wel gehoist
- Indien klasse niet expliciet erft ve andere klasse, erft de klasse van Object
    - Methodes gedefinieerd in Object kunnen steeds gebruikt worden
- Built-in javascript klassen kunnen extenden
- voorbeeld:
```javascript
class BlogDate extends Date {
    toBlogFormat() {
        const options = {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric'
    };
    return this.toLocaleDateString('en-NL', options);
    }
}

const blogDate = new BlogDate();
console.log(`Today's date: ${blogDate.toBlogFormat()}`);
```

## **Protypes**

> ### **Protypes**
Via prototype objecten kunnen objecten toestand en gedrag delen
- Object heeft zijn eigen properties en methodes
- Object heeft een `property_proto_`
- `__proto__` is prototype object via dewelke het object gedrag en toestand erft

De constructor & new
- JS functie
- `this`: properties en methodes definiëren
```javascript
function BlogEntry(body, date) {
    this.body = body;
    this.date = new Date();
}

class BlogEntry {
    constructor(body, date) {
        this.body = body;
        this.date = date;
    }
}
```
- functie aanroep voorafgegaan door `new` = creatie object
```javascript
function BlogEntry(body, date) {
    this.body = body;
    this.date = new Date();
}

const myBlogEntry = new BlogEntry('Prototypes rock!');
```
`_proto_`
```javascript
 const myBlogEntry = new BlogEntry('Prototypes rock!');
const anotherEntry = new BlogEntry('JavaScript rules!');
```
**Prototype chain**: ketting van prototype objecten (prototybe is object dus heeft zijn eigen prototype...)
- Wanneer naar een property van een object wordt gerefereerd, dan wordt eerst gezien of die property bestaat bij het object zelf, indien niet gevonden dan wordt er gezocht bij zijn prototype. Dit proces herhaalt zich recursief, tot het oer-object wordt bereikt: **Object**
- `Object.prototype` is de laatste schakel in de ketting
Alle properties/methodes die je toevoegt aan de **prototype property van een constructor functie** worden gedeeld door alle instanties die gecreëerd werden via die constructor functie
<br>

Wat kan je doen met Arrays? **Array.prototype**