# Sass SCSS curso

## Instalación de Sass globalmente

Antes de nada, es necesario tener Node.js instalado previamente para usar SASS.

## Página para realizar la instalación de Node

[página oficial de Node](https://nodejs.org/en)

## Instalación de SASS

SASS se instala con este comando.

```bash
npm install -g sass
```

**Ahora podemos comenzar a usar SASS**

## Uso de la etiqueta link

En nuestro archivo HTML dentro de public debemos de poner el siguiente link para que SCSS se pueda compilar a CSS.

```html
<link rel="stylesheet" href="/css/style.css" />
```

### Ejemplo 1

Imagina que nuestro archivo en SASS se llama nenu.scss. En nuestro archivo HTML, el link que vinculará el SASS con el CSS sería este.

```html
<link rel="stylesheet" href="/css/menu.css" />
```

### Ejemplo 2

si nuestro archivo se llama footer, tendríamos que poner este link

```html
<link rel="stylesheet" href="/css/footer.css" />
```

> Dependiendo de nuestro archivo SCSS nuestro archivo css tendra un nombre u otro

1. Después debemos de crear una carpeta **CSS** dentro de **public**

> **nota:** Los archivos de **SCSS** se crean en la carpeta **assets**.

## Comando para transformar Sass a CSS

```bash
 sass --watch src/assets:public/css
```

Tenemos que ejecutar este comando obligatoriamente para que pueda leer el archivo SCSS y convertirlo a CSS que lo entienden los navegadores sin esto nuestros archivos SCSS no funcionaran como hemos dicho antes los archivos Sass no los entiende el navegador por eso hay que referenciar a un archivo CSS tendremos que poner la ruta completa.

## ¿Qué hace este comando?

### primera función del comando scompilar los archivos scss

Este comando observa los cambios de nuestros archivos en SCSS que se encuentran en la carpeta **src/assets** y los compila a CSS en la carpeta **public/css**

Al ejecutar este comando, se crea la carpeta **CSS** dentro de **public** con archivos CSS. Cada archivo CSS tendrá el mismo nombre que el archivo SCSS correspondiente, pero con la extensión .css.

#### Ejemplos

- Si nuestro archivo se llama **style.scss** lo transformará a **style.css**
- Si nuestro archivo se llama **nav.scss** se transformará a **nav.css**

- Si nuestro archivo se llama **pepe.scss** se transformará a **pepe.css**

### segunda funcón del comando Archivo css.map

Los sourcemaps son una especie de mapa que vincula el código fuente original con el código generado.

Cuando se genera el código optimizado, también se crea un sourcemap, que contiene información sobre cómo se relaciona el código **CSS** generado.

**(con el código fuente original SCSS).** Esta información incluye los nombres de los archivos fuente, las líneas de código correspondientes y las columnas originales. Los source maps se generan generalmente en formato JSON o en formato de mapa de origen (.map).

### Conclusiones de estos 2 archivos.

- El primer archivo style.css compila nuestro archivo SCSS para que el navegador lo entienda.

- El segundo archivo **css.map** es como si fuera un mapa que nos ayuda a vincular el código SCSS con el código CSS.

#### Resultado del comando

![source map scss](/src/assets/comando_compilacion._resultado.png)

## Como definir variables

```scss
$color-primary: red;

h1 {
	color: $color-primary; // se define con $nombre variable
	background-color: black;
}
```

> Nota: si la ponemos al principio del documento será una variable global.

## Modularizar nuestro código

Podemos crear archivos sin que sean compilados a CSS para tener un código más organizado para ello debemos crear un archivo de **SCSS** con este formato **\_variables.scss** después en nuestro archivo SCSS principal debemos usar la palabra clave **@use** en este caso nuestro archivo principal no compilara el archivo que indiquemos en **@use**.

### Sintaxis

```scss
@use; _nombre de archivo sin extension;
```

### Ejemplo

Tenemos un caso especial imagina que definimos un archivo con todas las variables **\_variables.scss** nosotros usamos el módulo **@use \_nombre del archivo** y colocamos las variables en los atributos css que queramos, pero hay un problema no se aplicarán las variables porque nuestro módulo es de variables para poder utilizarlo usamos la siguiente sintaxis.

> Importante: Antes de nada definimos todas las variables que usaremos en nuestro archivo de variables **\_variables.scss** si nuestra aplicación es muy grande es aconsejable usar muchas variables

```scss
$color-primary: red; // Este es nuestro archivo de variables
$color-text: white;
```

Luego, en el archivo principal de **SCSS** usaremos el siguiente código.

```scss
@use "_variables"; // usamos el archivo de variables en nuestro scss principal

h1 {
 color: variables.$color-text; // para acceder a la variable ponemos el nombre del fichero.$variable

 background-color: variables.$color-primary;
}
```

#### veamos un paso a paso

1. En nuestro archivo \_variables.scss definimos las variables que necesitaremos
2. En nuestro archivo principal de SCSS usamos el módulo **@use_variables**
3. Por último, al atributo css que queremos aplicar las variables usamos esta sintaxis atributo css: archivo.$variable;

```scss
h1 {
	color: variables.$color-text; // para acceder a la variable ponemos el nombre del fichero.$variable
	background-color: variables.$color-primary;
}
```

## Nesting ejemplo

Imaginemos que tenemos un menú de navegación.

```html
<nav>
	<ul>
		<li>
			<a href="#home">home</a>
		</li>
		       
		<li>
			<a href="#usuario">usuario</a>
		</li>
	</ul>
</nav>
```

Queremos darle estilo a todo el menú, así lo hacemos con CSS.

```css
nav {
    background-color: rgb(190, 151, 227);
}

nav ul {
    display: flex;
    justify-content: center;
    gap: 1em;
}

ul li {
    list-style: none;
}

a {
    text-decoration: none;
}
```

Pero tardaremos mucho escribiendo todos esos estilos, podemos ahorrarnos muchas líneas de código usando el nesting

### Ejemplo nesting con codigo

```scss
nav {
    background-color: rgb(190, 151, 227);

    ul {
        display: flex;
        justify-content: center;
        gap: 1em;
    }

    li {
        list-style: none;
    }

    a {
        text-decoration: none;
    }
}
```

Ahora le decimos que el **nav** tendrá un color de fondo verde. Dentro del nav está el elemento **ul** con un **display flex  justify-content: center y un  gap: 1em;**
dentro de ese hay un elemento **li** le quitamos el estilo de lista por ultimo hay un elemento **a** con un **text-decoration: none.**

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

Como podemos ver, con el nesting, se entiende y se ve el código mucho más claro y es más fácil de leer, pero esta forma no es la más adecuada.

### Usando el selector padre &

#### Veamos un ejemplo

```html
<template>
	<nav class="nav__container">
		<a class="nav__link" href="ejemplo">ejemplo</a>  
		<a class="nav__link" href="ejemplo2">ejemplo</a> 
		<a class="nav__link" href="ejemplo3">ejemplo</a>
		<a class="nav__link" href="ejemplo4">ejemplo</a>    
	</nav>
</template>
```

Cundo nosotros estamos utilizando este selector **&** en nuestro archivo **SCSS** es una forma corta de escribir

```scss
.nav
```

> **⚠️ Es muy importante que nuestro elemento sea una clase o un id, de lo contrario nunca funcionará. ⚠️**

Pero nos ahorraremos más líneas todavía usando el selector padre **&** combinado con el nesting.

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
    &__link {
        text-decoration: none;
    }
}
```

Así se compila a css

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
.nav__link {
    text-decoration: none;
}
```

Como podemos observar, **&** hace referencia a **.nav** y los estilos del menú de navegación se visualizan muy rápido.

### Agregando algunas media queries.

Partimos de este ejemplo

```html
<template>
	<nav class="nav__container">
		<a class="nav__item" href="ejemplo">ejemplo</a>        
		<a class="nav__item" href="ejemplo2">ejemplo</a>        
		<a class="nav__item" href="ejemplo3">ejemplo</a>        
		<a class="nav__item--active" href="ejemplo4">ejemplo</a>    
	</nav>
</template>
```

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

La interpolación es una característica de SASS que nos permite colocar dentro de una variable el nombre de un selector y/o atributo.

1. Partimos de este ejemplo

```html
<template>
	<nav class="nav__container">
		 <a class="nav__item" href="ejemplo">ejemplo</a>        
		<a class="nav__item" href="ejemplo2">ejemplo</a>        
		<a class="nav__item" href="ejemplo3">ejemplo</a>        
		<a class="nav__item--active" href="ejemplo4">ejemplo</a>    
	</nav>
</template>
```

2. Debemos poner el nombre de la variable en donde hagamos la interpolación.

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

> Nota: Si en nuestro html tenemos 5 elementos el bucle solo recorrerá esos 5 elementos.

### Ejemplo

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

> Nota: Nos crea 50 selectores CSS pero ningún elemento HTML.

Aquí se muestra como se compilan los selectores circulo.

```css
selector-1 {
    color: black;
}

.selector-2 {
    color: black;
}

.selector-3 {
    color: black;
}

.selector-4 {
    color: black;
}

.selector-5 {
    color: black;
}

.selector-6 {
    color: black;
}

.selector-7 {
    color: black;
}

.selector-8 {
    color: black;
}

.selector-9 {
    color: black;
}

.selector-10 {
    color: black;
}

.selector-11 {
    color: black;
}

.selector-12 {
    color: black;
}

.selector-13 {
    color: black;
}

.selector-14 {
    color: black;
}

.selector-15 {
    color: black;
}

.selector-16 {
    color: black;
}
```

Así hasta llegar a 50

## mixins

Los mixin son como funciones en JavaScript

### Documentación de SASS

[documentacion SASS](https://sass-lang.com/documentation/)
