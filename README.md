# Sass SCSS curso

## Instalación de Sass globalmente 

Sass se instala con este comando. Es necesario tener Node.js instalado previamente.

```bash
npm install -g sass
```

**Así lo tendremos instalado** 

En nuestro archivo HTML dentro de public debemos de poner el siguiente link.

```html
<link rel="stylesheet" href="/css/style.css" />
```
1. Después debemos de crear una carpeta **CSS** dentro de **public** 
>> **nota:** Los archivos de **SCSS** se crean en la carpeta **assets**.


## Comando para transformar Sass a CSS

Tenemos que ejecutar este comando obligatoriamente para que pueda leer el archivo SCSS y convertirlo a CSS que lo entienden los navegadores sin esto nuestros archivos SCSS no funcionaran como hemos dicho antes los archivos Sass no los entiende el navegador por eso hay que referenciar a un archivo CSS tendremos que poner la ruta completa.

```bash
 sass --watch src/assets:public/css
```

Por supuesto que en nuestro archivo HTML tendremos que poner el link que hace referencia a nuestra hoja de estilos CSS que el navegador sí que entiende.

```html
<link rel="stylesheet" href="/css/style.css" />
```

## Como definir variables

```scss
$color-primary: red;

h1 {
    color: $color-primary; // se define con $nombre variable
    background-color: black;
}
```

si la ponemos al principio del documento será una variable global.

## Modularizar nuestro código

Podemos crear archivos sin que sean compilados a CSS para tener un código más organizado para ello debemos crear un archivo de **SCSS** con este formato **\_variables.scss** después en nuestro archivo Sass principal debemos usar la palabra clave **use** en este caso nuestro archivo principal va a compilar el archivo que indiquemos en **@use**.

### Sintaxis

```scss
@use; _nombre de archivo sin extension;
```

### veamos otro ejemplo

Tenemos un caso especial imagina que definimos un archivo con todas las variables **\_variables.scss** nosotros usamos él **use \_nombre del archivo** y colocamos las variables donde nosotros queramos, pero hay un problema no se aplicaran las variables porque nuestro módulo es de variables para poder utilizarlo usamos la siguiente sintaxis.

```scss
@use "_variables"; // usamos el archivo de variables en nuestro scss principal 

h1 {
    color: variables.$color-text; // para acceder a la variable ponemos el nombre del fichero.$variable
    background-color: variables.$color-primary;
}
```

#### veamos un paso a paso

1. Creamos nuestro archivo con esta sintaxis **\_nombreArchivo.scss**.
   2. Ponemos lo que queremos en el archivo.
2. Después nos vamos al archivo principal de SCSS y usamos la palabra clave @use '\_nombreArchivo.scss'
3. Por otra parte, si hacemos un archivo de variables **\_variables.scss** debemos de utilizar el @use **nombreArchivo.scss** para que funcione debemos usar el siguiente código.

```scss
h1 {
    color: variables.$color-text; // para acceder a la variable ponemos el nombre del fichero.$variable
    background-color: variables.$color-primary;
}
```

## Nesting ejemplo

Imaginemos que tenemos un menú de navegación y queremos acceder al elemento **a** esta seria la forma sin nesting.

```scss
nav ul li a {
    text-decoration: none; // manera tradicional
}
```

### Ejemplo nesting con codigo

```scss
nav {
    background-color: blue;
    padding: 1em;
    ul {
        color: black;
    }
    li {
        list-style: none;
    }
    a {
        text-decoration: none;
        color: red;
    }
}
```

Ahora le decimos que el **nav** tendrá un color de fondo con un padding dentro del nav está el elemento **ul** con un color negro
dentro de ese un elemento **li** le quitamos el estilo de lista y dentro del él está el elemento **a** con un text-decoration y un color rojo.

De esta manera, nuestro SCSS quedará mucho más limpio.

### veamos como lo transforma a CSS

```css
nav {
    color: blue;
}

nav {
    background-color: blue;
    padding: 1em;
}
nav ul {
    color: black;
}
nav li {
    list-style: none;
}
nav a {
    text-decoration: none;
    color: red;
}
```

Como podemos ver, con el nesting, se entiende y se ve el código mucho más claro y es más fácil de leer, pero esta forma no es la adecuada.

### Usando el selector padre &

#### Veamos un ejemplo

```vue
<template>
    <nav class="nav__container">
        <a href="ejemplo">ejemplo</a>
        <a href="ejemplo2">ejemplo</a>
        <a href="ejemplo3">ejemplo</a>
        <a href="ejemplo4">ejemplo</a>
    </nav>
</template>
```

Cundo nosotros estamos utilizando este selector **&** en nuestro archivo SCSS es una forma corta de escribir

```scss
.nav
```

> **⚠️ Es muy importante que nuestro elemento sea una clase o un id, de lo contrario nunca funcionará. ⚠️**

Esta sería la mejor manera de escribirlo

```scss
.nav {
    background-color: cadetblue;

    &__container {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 1em;
        width: 100%;
        height: 80px;
        background-color: chocolate;
    }
}
```

Así quedaría compilado en CSS

```css
.nav {
    background-color: cadetblue;
}
.nav__container {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 1em;
    width: 100%;
    height: 80px;
    background-color: chocolate;
}
```

como podemos observar **&** hace referencia a **.nav**

### Agregando media query

```scss
.nav {
    background-color: cadetblue;

    &__container {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 1em;
        width: 100%;
        height: 80px;
        background-color: rgb(84, 84, 220);
    }
    &__item {
        text-decoration: none;
        color: rgb(29, 29, 35);
        font-size: 1em;
    }

    &__item:hover {
        color: white;
    }

    @media only screen and (min-width: 800px) {
        &__container {
            display: flex;
            justify-content: end;
        }
    }
}
```

## interpolar variables

Esto se aplica a selectores y propiedades, etc. como por ejemplo un color o un selector after

 Primero, debemos definir el nombre de la variable.

```scss
$display: "display";
```

### Este sería un ejemplo mal hecho

```scss
.nav {
    background-color: cadetblue;

    &__container {
        $display: flex;
        justify-content: center;
        align-items: center;
        gap: 1em;
        width: 100%;
        height: 80px;
        background-color: rgb(84, 84, 220);
    }
}
```

**Resultado**

![Interpolar ejemplo mal hecho](/src/assets/interpolacion_bad_example.png)

### Veamos como se interpola una variable correctamente

```scss
.nav {
    background-color: cadetblue;

    &__container {
        #{$display}: flex;
        justify-content: center;
        align-items: center;
        gap: 1em;
        width: 100%;
        height: 80px;
        background-color: rgb(84, 84, 220);
    }
}
```
![interpolar ejemplo bien hecho](/src/assets/interpolacion_good_example.png)
## bucles en SASS

En Sass podemos hacer bucles.

Veamos el bucle for con un ejemplo de código con una animación.

**este es el HTML**

```html
<section>
    <div class="circle circle1"></div>
    <div class="circle circle2"></div>
    <div class="circle circle3"></div>
    <div class="circle circle4"></div>
    <div class="circle circle5"></div>
</section>
```

```scss
@use "_variables";
.circle {
    background-color: variables.$color-primary;
    border-radius: 50px;
    width: 300px;
    height: 300px;
    animation: 2s linear infinite alternate;
}

@for $iterador from 1 through 50 {
    .selector-#{$iterador} {
        color: black;
    }
}
```

### veamos un paso a paso

1. Pondremos la regla @for.
2. Definimos una variable con el nombre que queramos esta variable irá recorriendo cada iteración del for primero será 1 luego 2 así hasta el número que nosotros le indiquemos en este ejemplo llegara hasta 50.

3. Podremos desde que número empieza con la palabra clave **from,** en este ejemplo es 1 y el número en el que acaba. Con la palabra clave **through** que en este ejemplo es 50, **esto sería, la condición** oye empieza en 1 y acaba en 50.
4. Ponemos a las clases o elementos que afectara en este ejemplo es **.selector-** interpolamos la variable para que en cada vuelta del for sea 1 2 3 hasta llegar a 50.
5. ponemos la propiedad a la que afectara.


## mixins

Los mixin son como funciones en JavaScript


### Documentación de SASS

[documentacion SASS](https://sass-lang.com/documentation/)