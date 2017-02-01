# Código limpio Javascript

## Tabla de conteindo
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