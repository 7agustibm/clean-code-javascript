# Código limpio Javascript

## Tabla de contenido
  1. [Introducción](#introduccion)
  2. [Variables](#variables)
  3. [Funciones](#funciones)
  4. [Objetos y Estructura de Datos](#objetos-y-estructura-de-datos)
  5. [Classes](#classes)
  6. [Testing](#testing)
  7. [Concurrencia](#concurrencia)
  8. [Manejo de Errores](#manejo-de-errores)
  9. [Formateo](#Formateo)
  10. [Comentarios](#comentarios)
  11. [Traducciones](#raducciones)

## Introducción
  ![Imagen humorística de la estimación de la calidad del software como el numero de quejas que gritas cuando lees un código](http://www.osnews.com/images/comics/wtfm.jpg)

Los principios de la ingenería del Software, del libro [*Código Limpio*](https://www.amazon.es/Código-Limpio-desarrollo-software-Programación/dp/8441532109) de Robert C. Martin,
adaptados para Javascript. Esto no es una guía de estilos. Esto es una guía para generar código Javascript  legible, reutilizable y refactorizable.

No todos los principios deben ser seguidos estrictamente, y menos aún serán universalmente acordados.Esto son directrices y nada más, pero los autores de *Código Limpio* han sido programadores durante muchos años y acumulando experiencia colectivas.

Nuestra oficio de ingeniería de software solo tiene un poco más de 50 años, y todavía estamos 
aprendiendo mucho. Cuando la arquitectura de software se tan vieja como la arquitectura, quizás
entonces nosotros tendremos reglas más difíciles de seguir. Por ahora, permite que estas directrices 
te sirva como piedra angular para evaluar la calidad del código de Javascript que tu y tu equipo
producen.

Una cosa más: sabiendo que éstos no le harán inmediatamente un mejor developer del software, y el 
trabajar con ellos por muchos años no significa que usted no incurrirá en equivocaciones. Cada 
trozo de código que comienza como un primer borrador, es como la arcilla mojada que consigue 
formada en su forma final. Finalmente, pulimos las imperfecciones cuando la revisamos 
con nuestros compañeros. No te autocastigues por los primeros borradores que deberas de mejorar.
¡Mejorar el código en su lugar!

## **Variables**
### Utilizar nombres de variables significativos y pronunciables

**Mal hecho:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Bien hecho:**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Utilizar el mismo vacabulario para el mismo tipo de variable

**Mal hecho:**
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Bien hecho:**
```javascript
getUser();
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Utilizar nombre que se pueden buscar 
Leeremos más código de lo que escribiremos. Es importante que el código que vamos 
a escribir se legible y entendible.
Por *no* nombrar variables que terminan siendo significativas para entender 
nuestro programa, lastimamos a nuestros lectores.
Haz que tus nombres d variables se pueden entender. Herramientas como 
[buddy.js](https://github.com/danielstjules/buddy.js) y
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
pueden ayudarte a identificar constantes sin nombres.

**Mal hecho:**
```javascript
// ¿Qué quiere decir 86400000?
setTimeout(blastOff, 86400000);

```

**Bien hecho:**
```javascript
// Declara como globales `const` en mayusculas.
const MILISEGUNDOS_EN_UN_DIA = 86400000;

setTimeout(blastOff, MILISEGUNDOS_EN_UN_DIA);

```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Usar variables explicativas
**Mal hecho:**
```javascript
const direccion = 'One Infinite Loop, Cupertino 95014';
const codigoZipCiudadRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(direccion.match(codigoZipCiudadRegex)[1], direccion.match(codigoZipCiudadRegex)[2]);
```

**Bien hecho:**
```javascript
const direccion = 'One Infinite Loop, Cupertino 95014';
const codigoZipCiudadRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = direccion.match(codigoZipCiudadRegex) || [];
saveCityZipCode(city, zipCode);
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Evite la asignación mental
Explicito es mejor que implícito.

**Mal hecho:**
```javascript
const ciudedes = ['Austin', 'New York', 'San Francisco'];
ciudedes.forEach((c) => {
  hacerAlgo();
  hacerAlgoOtraVez();
  // ...
  // ...
  // ...
  // Espera, ¿qué es `c`?
  dispatch(c);
});
```

**Bien hecho:**
```javascript
const ciudedes = ['Austin', 'New York', 'San Francisco'];
ciudedes.forEach((ciudad) => {
  hacerAlgo();
  hacerAlgoOtraVez();
  // ...
  // ...
  // ...
  dispatch(ciudad);
});
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### No agregue contexto innecesario
Si su nombre de clase/objeto le dice algo, no lo repita en su
nombre de la variable.

**Mal hecho:**
```javascript
const Coche = {
  fabricanteCoche: 'Honda',
  modeloCoche: 'Accord',
  colorCoche: 'Azul'
};

function pintarCoche(coche) {
  coche.colorCoche = 'Rojo';
}
```

**Bien hecho:**
```javascript
const Coche = {
  fabricante: 'Honda',
  modelo: 'Accord',
  color: 'Azul'
};

function pintarCoche(coche) {
  coche.color = 'rojo';
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Usar argumentos predeterminados en lugar de cortocircuitos o condicionales
Los argumentos por defecto son a menudo más limpios que los cortocircuitos. Tenga en cuenta 
que si los utiliza, su función sólo proporcionará valores predeterminados para los argumentos 
`undefined`. Otros valores falsos como `''`, `""`, `false`, `null`, `0`, y `NaN` no serán 
reemplazados por un valor predeterminado.


**Mal hecho:**
```javascript
function crearMicrobrewery(nombre) {
  const breweryNombre = nombre || 'Hipster Brew Co.';
  // ...
}

```

**Bien hecho:**
```javascript
function crearMicrobrewery(breweryNombre = 'Hipster Brew Co.') {
  // ...
}

```
**[⬆ Volver al inició](#tabla-de-contenido)**

## **Funciones**
### Argumentos de la función (2 o menos idealmente)
Limitar la cantidad de parámetros de función es increíblemente importante porque 
facilita la prueba de su función. Tener más de tres conduce a una explosión 
combinatoria donde hay que probar toneladas de casos diferentes con cada argumento separado.


Uno o dos argumentos es el caso ideal, y tres deben ser evitados si es posible.
Cualquier cosa más que eso debería consolidarse. Por lo general, si tiene más de dos 
argumentos entonces su función está tratando de hacer demasiado. En los casos en que no lo sea,
 la mayoría de las veces un objeto de nivel superior será suficiente como argumento.

Dado que JavaScript te permite hacer objetos sobre la marcha, sin una gran cantidad de elementos 
de la clase, puedes usar un objeto si te encuentras necesitando muchos argumentos.

Para hacer obvio qué propiedades espera la función, puede usar la sintaxis desestructora ES6. 
Esto tiene algunas ventajas:

1. Cuando alguien mira la firma de la función, inmediatamente está claro qué propiedades
 están siendo utilizadas.
2. La desestructuración también clona los valores primitivos especificados del objeto
 de argumento pasado en la función. Esto puede ayudar a prevenir los efectos secundarios.
  Nota: los objetos y matrices desestructurados del objeto de argumento NO se clonan.
3. Linters puede advertirle acerca de las propiedades no utilizadas, lo que sería 
imposible sin desestructuración.

**Mal hecho:**
```javascript
function crearMenu(titulo, cuerpo, textoBoton, cancellable) {
  // ...
}
```

**Bien hecho:**
```javascript
function crearMenu({ titulo, cuerpo, textoBoton, cancellable }) {
  // ...
}

crearMenu({
  titulo: 'Foo',
  cuerpo: 'Bar',
  textoBoton: 'Baz',
  cancellable: true
});
```
**[⬆ Volver al inició](#tabla-de-contenido)**


### Las funciones deben hacer una cosa
Esta es con mucho la regla más importante en ingeniería de software. Cuando las funciones 
hacen más de una cosa, son más difíciles de componer, probar y razonar.
Cuando se puede aislar una función a una sola acción, se puede refactorizar fácilmente 
y su código sera mucho más limpio y legible. Si utilizas solamente esta guía,
 estará por delante de muchos desarrolladores.

**Mal hecho:**
```javascript
function emailClients(clientes) {
  clientes.forEach((cliente) => {
    const clientRecord = database.lookup(cliente);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Bien hecho:**
```javascript
function emailClients(clientes) {
  clientes
    .filter(isClientActive)
    .forEach(email);
}

function isClientActive(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Los nombres de las funciones deben decir lo que hacen

**Mal hecho:**
```javascript
function añadirFecha(fecha, month) {
  // ...
}

const fecha = new Date();

// It's hard to to tell from the function name what is added
añadirFecha(fecha, 1);
```

**Bien hecho:**
```javascript
function añadirMesFecha(mes, fecha) {
  // ...
}

const fecha = new Date();
añadirMesFecha(1, fecha);
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Las funciones deben ser sólo un nivel de abstracción
Cuando tienes más de un nivel de abstracción, tu función suele estar haciendo 
demasiado. La división de funciones conduce a la reutilización y pruebas más fáciles.

**Mal hecho:**
```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // ...
  });

  ast.forEach((node) => {
    // ...
  });
}
```

**Bien hecho:**
```javascript
function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function lexer(tokens) {
  const ast = [];
  tokens.forEach((token) => {
    ast.push( /* ... */ );
  });

  return ast;
}

function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const ast = lexer(tokens);
  ast.forEach((node) => {
    // parse...
  });
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Eliminar código duplicado
Haga lo mejor posible para evitar el código duplicado. El código duplicado 
es malo porque significa que hay más de un lugar para alterar algo si 
necesitas cambiar alguna lógica.

Imagínese si dirige un restaurante y realiza un seguimiento de su inventario: 
todos sus tomates, cebollas, ajo, especias, etc. Si tiene varias listas que mantener esto,
 entonces tienen que actualizarlos todos cuando se sirve un plato con tomates en ellos. 
 Si sólo tiene una lista, sólo hay un lugar para actualizar!

A menudo tienes código duplicado porque tienes dos o más cosas ligeramente diferentes, que 
comparten mucho en común, pero sus diferencias te obligan a tener dos o más funciones separadas 
que hacen muchas de las mismas cosas. Eliminar código duplicado significa crear una abstracción 
que puede manejar este conjunto de cosas diferentes con sólo una función/módulo/clase.

Conseguir una correcta abstracción es crítico, eso es porqué usted debe seguir los principios SOLID 
establecidos en la sección *Classes*. Las malas abstracciones pueden ser peores que el código duplicado,
 así que ten cuidado! Habiendo dicho esto, si usted puede hacer una buena abstracción, hazlo! No te repitas,
  de lo contrario te encontrarás actualizando varios lugares cada vez que quieras cambiar una cosa.

**Mal hecho:**
```javascript
function showDeveloperList(developers) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Bien hecho:**
```javascript
function showEmployeeList(employees) {
  employees.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    let portfolio = employee.getGithubLink();

    if (employee.type === 'manager') {
      portfolio = employee.getMBAProjects();
    }

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Establecer objetos predeterminados con Object.assign

**Mal hecho:**
```javascript
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable === undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Bien hecho:**
```javascript
const menuConfig = {
  title: 'Order',
  // El usuario no incluyó la clave "cuerpo"
  buttonText: 'Send',
  cancellable: true
};

function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // config ahora es igual: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```
**[⬆ Volver al inició](#tabla-de-contenido)**


### No utilice indicadores como parámetros de función
Los indicadores indican a su usuario que esta función hace más de una cosa. Las funciones 
deben hacer una cosa. Divida sus funciones si están siguiendo diferentes rutas de código 
basadas en un booleano.

**Mal hecho:**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Bien hecho:**
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Evitar efectos secundarios (parte 1)
Una función produce un efecto secundario si hace algo más que tomar un valor y devolver otro 
valor o valores. Un efecto secundario podría ser la escritura en un archivo, la modificación 
de alguna variable global.

Ahora, usted necesita tener efectos secundarios en un programa de vez en cuando. Al igual que en el ejemplo anterior,
es posible que necesite escribir en un archivo. Lo que usted quiere hacer es centralizar donde usted está haciendo esto.
 No tiene varias funciones y clases que escriben en un archivo en particular.
 Tener un servicio que lo hace. Uno y solo uno.

El punto principal es evitar las trampas comunes como compartir el estado entre los objetos sin ninguna estructura, 
usando tipos de datos mutables que pueden ser escritos por cualquier cosa, y no centralizando donde ocurren 
sus efectos secundarios. Si usted puede hacer esto, usted será más feliz que la gran mayoría de otros programadores.

**Mal hecho:**
```javascript
// Variable global referenciada por la siguiente función.
// Si tuviéramos otra función que usara este nombre, ahora sería una matriz y podría romperla.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Bien hecho:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Evitar efectos secundarios (parte 2)

En Javascript, los primitivos son pasado por valor y los objetos/arrays son 
pasado por referencia. En el caso de los objetos y los arrays, si nuestra 
función crear un cambio en un array del carrito de la compra, por ejemplo,
añadir un ítem para comprar, entonces ninguna función que use este array `cart`
debería ser afectado por este cambio. Eso puede ser genial, sin embargo también 
eso puede ser malo. Vamos a imaginar una situación errónea:

El usuario presiona el "Comprar", el botón al cual llama a la función `purchase`
que genera una petición de red y envía el array `cart` al servidor. Porque debido
una mala conexión, la función `purchase` tiene que reintentar la petición. Ahora,
¿Qué pasa si entretanto el usuario hace clic de forma accidental en el botón 
"Añadir al carro" de un elemento que en realidad no quieren antes de que 
comience la solicitud de red? Si esto pasa y la petición de conexión empieza, 
cuando esta función de compra envié por accidente el ítem porque tenia una 
referencia en el array del carrito que la función `addItemToCart` modifico este 
añadió un elemento que no queremos.

Una buena solución seria que la función `addITemToCart` siempre hiciera un copia de 
`cart`, editarlo, y devolver el clon. Esto garantiza que ninguna otra función que 
se mantenga en una referencia del carrito de compras se verá afectada por cualquier cambio.


Dos advertencias a mencionar a este enfoque:
  1. Puede haber casos en los que realmente desea modificar el objeto de entrada, pero 
  cuando adopta esta práctica de programación encontrará que esos casos son bastante raros. 
  ¡La mayoría de las cosas pueden ser refactorizadas para no tener efectos secundarios!

  2. Clonar objetos grandes puedes ser costosos en terminos de performance. Por suerte,
  esto no es un gran problema en la practica porque hay [buenas librerias](https://facebook.github.io/immutable-js/)
  que permiten que este tipo de enfoque de programación sea rápido y no use tanta memoria como si lo hicieramos 
  nosotros manualmente.

**Mal hecho:**
```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Bien hecho:**
```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date : Date.now() }];
};
```

**[⬆ Volver al inició](#tabla-de-contenido)**

### Don't write to global functions
Polluting globals is a bad practice in JavaScript because you could clash with another
library and the user of your API would be none-the-wiser until they get an
exception in production. Let's think about an example: what if you wanted to
extend JavaScript's native Array method to have a `diff` method that could
show the difference between two arrays? You could write your new function
to the `Array.prototype`, but it could clash with another library that tried
to do the same thing. What if that other library was just using `diff` to find
the difference between the first and last elements of an array? This is why it
would be much better to just use ES2015/ES6 classes and simply extend the `Array` global.

**Mal hecho:**
```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Bien hecho:**
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Favor functional programming over imperative programming
JavaScript isn't a functional language in the way that Haskell is, but it has
a functional flavor to it. Functional languages are cleaner and easier to test.
Favor this style of programming when you can.

**Mal hecho:**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Bien hecho:**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const INITIAL_VALUE = 0;

const totalOutput = programmerOutput
  .map((programmer) => programmer.linesOfCode)
  .reduce((acc, linesOfCode) => acc + linesOfCode, INITIAL_VALUE);
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Encapsulate conditionals

**Mal hecho:**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**Bien hecho:**
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Avoid negative conditionals

**Mal hecho:**
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Bien hecho:**
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Avoid conditionals
This seems like an impossible task. Upon first hearing this, most people say,
"how am I supposed to do anything without an `if` statement?" The answer is that
you can use polymorphism to achieve the same task in many cases. The second
question is usually, "well that's great but why would I want to do that?" The
answer is a previous clean code concept we learned: a function should only do
one thing. When you have classes and functions that have `if` statements, you
are telling your user that your function does more than one thing. Remember,
just do one thing.

**Mal hecho:**
```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Bien hecho:**
```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Avoid type-checking (part 1)
JavaScript is untyped, which means your functions can take any type of argument.
Sometimes you are bitten by this freedom and it becomes tempting to do
type-checking in your functions. There are many ways to avoid having to do this.
The first thing to consider is consistent APIs.

**Mal hecho:**
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**Bien hecho:**
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Avoid type-checking (part 2)
If you are working with basic primitive values like strings, integers, and arrays,
and you can't use polymorphism but you still feel the need to type-check,
you should consider using TypeScript. It is an excellent alternative to normal
JavaScript, as it provides you with static typing on top of standard JavaScript
syntax. The problem with manually type-checking normal JavaScript is that
doing it well requires so much extra verbiage that the faux "type-safety" you get
doesn't make up for the lost readability. Keep your JavaScript clean, write
good tests, and have good code reviews. Otherwise, do all of that but with
TypeScript (which, like I said, is a great alternative!).

**Mal hecho:**
```javascript
function combine(val1, val2) {
  if (typeof val1 === 'number' && typeof val2 === 'number' ||
      typeof val1 === 'string' && typeof val2 === 'string') {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**Bien hecho:**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Don't over-optimize
Modern browsers do a lot of optimization under-the-hood at runtime. A lot of
times, if you are optimizing then you are just wasting your time. [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
for seeing where optimization is lacking. Target those in the meantime, until
they are fixed if they can be.

**Mal hecho:**
```javascript

// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Bien hecho:**
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```
**[⬆ Volver al inició](#tabla-de-contenido)**

### Remove dead code
Dead code is just as bad as duplicate code. There's no reason to keep it in
your codebase. If it's not being called, get rid of it! It will still be safe
in your version history if you still need it.

**Mal hecho:**
```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');

```

**Bien hecho:**
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
**[⬆ Volver al inició](#tabla-de-contenido)**