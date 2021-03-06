# Guia-Instalación-Antlr4- v4 Español
Esta guia te mostrará como instalar Antlr4 un potente generador de parser para todas las plataformas.

Es la nueva versión de ANTLR una mejora de las versiones antiguas es el hecho de poder escribir gramaticas mas naturales y estandarizadas como lo realiza yacc! Mira [prefacio del libro de ANTLR v4](http://media.pragprog.com/titles/tpantlr2/preface.pdf) como una guía mas detallada.

## Descripción

ANTLR es realmente dos cosas: una herramienta que traduce su gramática a un parser/lexer en Java (u otro lenguaje de programación) y las rutinas necesarias para los analizadores / lexers generados. Incluso si está utilizando el complemento ANTLR Intellij o ANTLRWorks para ejecutar la herramienta ANTLR, el código generado seguirá necesitando la biblioteca con las rutinas.

## Instalación


Antes de comenzar con la instalación es necesario descargar lo siguiente:
<br>
Java JDK [http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
<br>
antlr-4.5.3-complete.jar [http://www.antlr.org/download.html](http://www.antlr.org/download.html)


### UNIX

0. Instalar Java JDK (versión 1.6 o superior)
1. Descargar
```
$ cd /Users/tu_Usuario/Library
$ curl -O http://www.antlr.org/download/antlr-4.6-complete.jar
```
O descargalo desde el navegador desde:
    [http://www.antlr.org/download.html](http://www.antlr.org/download.html)
y ponlo en algun lugar racional como `/Users/tu_Usuario/Library`.
2. Agrega `antlr-4.6-complete.jar` a tu `CLASSPATH`:
```
$ export CLASSPATH=".:/Users/Library/antlr-4.6-complete.jar:$CLASSPATH"
```

3. Create alias para ANTLR, y para `TestRig`.
```
$ alias antlr4='java -Xmx500M -cp "/usr/local/lib/antlr-4.5.3-complete.jar:$CLASSPATH" org.antlr.v4.Tool'
$ alias grun='java org.antlr.v4.gui.TestRig'
```
![](images/Instalacionunix.png)

### WINDOWS


0. Instalar Java SE Development Kit 8u121(version 1.6 o superior) desde [http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
  * Considerar que el destino de ubicación(Drive) debe ser el mismo que el del sistema operativo.
1. Descargar antlr-4.6-complete.jar (o cualquier version) de [http://www.antlr.org/download/](http://www.antlr.org/download/)
Guardar en el directorio para librerias de terceros, como: `C:\Javalib`
2. Añadir `antlr-4.6-complete.jar` al PATH, de la siguiente manera:
  * Ir a  "Cuentas de Usuario"> Cambiar las variables de entorno.
  ![](images/antlr2.PNG)
  * Crear o añadir una variable `PATH` para antlr `C:\Javalib\antlr-4.6.complete.jar` y para el jdk `C:\Program Files\jdk.1.8.0_111\bin`

![](images/antlr3.PNG)

3. Crear atajos para los comandos de las herramientas ATNRL y TestRig, usandos archivos batch o comandos doskey:

  * Archivo batch con el siguiente contenido (en el mismo direcorio agregado de 'PATH) `antlr4.bat`
```
java org.antlr.v4.Tool %*
```
  * Archivo batch con el siguiente contenido (en el mismo direcorio agregado de 'PATH) `grun.bat`
```
java org.antlr.v4.gui.TestRig %*
```
![](images/antlr4.PNG)
  * O, usando comandos doskey:
```
doskey antlr4=java org.antlr.v4.Tool $*
doskey grun =java org.antlr.v4.gui.TestRig $*
```

### Probando la instalación en WINDOWS con un Ejemplo

En el directorio `CLASSPATH` `C:\Javalib` ejecutar el comando  :
* Siempre para empezar el uso, en la terminal de comandos (CMD) , realizar un set del PATH antlr:
```
SET CLASSPATH=.;C:\Javalib\antlr-4.6-complete.jar;%CLASSPATH%
```
* Ejectuar la siguiente instrucción para verificar la instalación
```
$ C:\Javalib>antlr4
ANTLR Parser Generator Version 4.6
-o ___ specify output directory where all output is generated
-lib ___ specify location of .tokens files
...
```
![](images/antlr5.PNG)

* En la misma  carpeta `CLASSPATH` colocar la siguiente gramática dentro de un archivo `Hello.g4`:

```
// Define a grammar called Hello
grammar Hello;
r  : 'hello' ID ;         // match keyword hello followed by an identifier
ID : [a-z]+ ;             // match lower-case identifiers
WS : [ \t\r\n]+ -> skip ; // skip spaces, tabs, newlines
```

Para ejectuar la herramienta ANTLR en la consola (CMD):

```
$ cd /Javalib
$ SET CLASSPATH=.;C:\Javalib\antlr-4.6-complete.jar;%CLASSPATH%
$ antlr4 Hello.g4
$ javac Hello*.java
```

Ahora para la prueba:

```
$ grun Hello r -tree
hello parrt
^Z
(r hello parrt)
// ^Z significa en Windows una salida(CTRL+Z > ENTER)). La opción -tree imprime el  árbol de parseo en notación LISP.
$ grun Hello r -gui
hello parrt
^Z
```
![](images/antlr6.PNG)

Esto genera una ventana que muestra la regla `r` unida a la palabra clave `hello` seguida del identificador `parrt`.

![](images/antlr7.PNG)

### Probando la instalación UNIX

La primera forma es poniendo el comando org.antlr.v4.Tool directamente:

```
$ java org.antlr.v4.Tool
ANTLR Parser Generator Version 4.6
-o ___ specify output directory where all output is generated
-lib ___ specify location of .tokens files
...
```

o usa la opción -jar en java:

```
$ java -jar /Users/Library/antlr-4.6-complete.jar
ANTLR Parser Generator Version 4.6
-o ___ specify output directory where all output is generated
-lib ___ specify location of .tokens files
...
```
![](images/Instalacionunix2.png)

## Un primer ejemplo UNIX gramática JSON

En un directorio temporal, colocar la siguiente gramática dentro de un archivo llamado JSON.g4:

```
/** Taken from "The Definitive ANTLR 4 Reference" by Terence Parr */

// Derived from http://json.org
grammar JSON;

json
   : value
   ;

obj
   : '{' pair (',' pair)* '}'
   | '{' '}'
   ;

pair
   : STRING ':' value
   ;

array
   : '[' value (',' value)* ']'
   | '[' ']'
   ;

value
   : STRING
   | NUMBER
   | obj
   | array
   | 'true'
   | 'false'
   | 'null'
   ;


STRING
   : '"' (ESC | ~ ["\\])* '"'
   ;
fragment ESC
   : '\\' (["\\/bfnrt] | UNICODE)
   ;
fragment UNICODE
   : 'u' HEX HEX HEX HEX
   ;
fragment HEX
   : [0-9a-fA-F]
   ;
NUMBER
   : '-'? INT '.' [0-9] + EXP? | '-'? INT EXP | '-'? INT
   ;
fragment INT
   : '0' | [1-9] [0-9]*
   ;
// no leading zeros
fragment EXP
   : [Ee] [+\-]? INT
   ;
// \- since - means "range" inside [...]
WS
   : [ \t\n\r] + -> skip
   ;
```
![](images/Instalacionunix6.png)

Ahora corre la herramienta ANTLR:

```
$ cd /tmp
$ antlr4 JSON.g4
$ javac JSON*.java
```
![](images/Instalacionunix3.1.png)

Ahora lo probaremos:

```
$ grun JSON json -tree
{
    "glossary": {
        "title": "example glossary",
		"GlossDiv": {
            "title": "S",
			"GlossList": {
                "GlossEntry": {
                    "ID": "SGML",
					"SortAs": "SGML",
					"GlossTerm": "Standard Generalized Markup Language",
					"Acronym": "SGML",
					"Abbrev": "ISO 8879:1986",
					"GlossDef": {
                        "para": "A meta-markup language, used to create markup languages such as DocBook.",
						"GlossSeeAlso": ["GML", "XML"]
                    },
					"GlossSee": "markup"
                }
            }
        }
    }
}
^D
(r hello parrt)
(That ^D means EOF on unix; it's ^Z in Windows.) The -tree option prints the parse tree in LISP notation.
It's nicer to look at parse trees visually.
$ grun JSON json r -gui
{
    "glossary": {
        "title": "example glossary",
		"GlossDiv": {
            "title": "S",
			"GlossList": {
                "GlossEntry": {
                    "ID": "SGML",
					"SortAs": "SGML",
					"GlossTerm": "Standard Generalized Markup Language",
					"Acronym": "SGML",
					"Abbrev": "ISO 8879:1986",
					"GlossDef": {
                        "para": "A meta-markup language, used to create markup languages such as DocBook.",
						"GlossSeeAlso": ["GML", "XML"]
                    },
					"GlossSee": "markup"
                }
            }
        }
    }
}
^D
```
![](images/Instalacionunix5.1.png)

![](images/Instalacionunix5.2.png)
Esto genera una ventana que muestra la regla `JSON`y sus valores, se puede observar el arbol de parseo para el JSON ingresado con todos los identificadores correspondientes.

![](images/Instalacionunix4.1.png)

## Conclusiones y recomendaciones

* En este trabajo, presentamos ANTLR, el generador de parser de PCCTS. En primer lugar, ANTLR es una herramienta práctica, programable-amistosa con muchas características convenientes. ANTLR integra la especificación del análisis léxico y sintáctico, soporta la notación ampliada de BNF, puede construir automáticamente sintaxis de árboles abstractos, informa y recupera de errores de sintaxis automáticamente, y proporciona flexibilidad semántica significativa. ANTLR genera analizadores de descensos recursivos rápidos, compactos y legibles en C o C ++ que son fáciles de integrar con otras aplicaciones.
* ANTLR utiliza una nueva estrategia de análisis que permite desarrollar gramáticas naturales y fáciles de leer para lenguajes difíciles como C ++.
Los predicados permiten la semántica arbitraria y la información sintáctica para dirigir el análisis.
* ANTLR es un software gratuito de dominio público.

## Libro con codigo fuente

El libro posee muchos ejemplos que podrian ser utiles. Puedes descargarlos gratis de:

[http://pragprog.com/titles/tpantlr2/source_code](http://pragprog.com/titles/tpantlr2/source_code)

Tambien, en este repositorio de github hay una larga colección de gramaticas para la version 4:

[https://github.com/antlr/grammars-v4](https://github.com/antlr/grammars-v4)

## Autores de la Guía

Andrés Asimbaya [github-ANDRES2527](https://github.com/ANDRES2527) <br>
Patricio Perez [github-elludenf](https://github.com/Elludenf)
