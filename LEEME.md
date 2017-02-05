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