# NYTimes - Guía de estilo de Objective-C

Esta guía de estilo define los líneamientos de desarrollo para los equipos de iOS en The New York Times. Agradecemos sus comentarios en los [issues](https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls) y [tweets](https://twitter.com/nytimesmobile). También, [estamos contratando](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY-10001/73366300/).

Gracias a todos [nuestros contribuyentes](https://github.com/NYTimes/objective-c-style-guide/contributors).

## Introducción

Aquí puedes encontrar algunos de los documentos de Apple sobre las guías de estudio. Si algo no se menciona aquí, probablemente estará cubierto con gran detalle en alguno de éstos:

* [El lenguaje de programación Objective -C](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Guía de fundamentos de Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [líneamientos de desarrollo para Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [Guía de desarrollo de aplicaciones iOS](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Contenido

* [Sintaxis Dot Notation](#sintaxis-dot-notation)
* [Espaciado](#espaciado)
* [Condicionales](#condicionales)
  * [Operador ternario](#operador-ternario)
* [Manejo de errores](#manejo-de-errores)
* [Métodos](#metodos)
* [Variables](#variables)
* [Nombres](#naming)
* [Comentarios](#comments)
* [Init y Dealloc](#init-and-dealloc)
* [Literales](#literals)
* [Funciones CGRect](#cgrect-functions)
* [Constantes](#constants)
* [Tipos enumerados](#enumerated-types)
* [Máscara de bits](#bitmasks)
* [Propiedades privadas](#private-properties)
* [Nombres de imágenes](#image-naming)
* [Booleanos](#booleans)
* [Singletons](#singletons)
* [Imports](#imports)
* [Proyecto de Xcode](#xcode-project)

## Sintaxis Dot Notation

Hay que utilizar punto **siempre** que se acceda o altere alguna propiedad. En todos los otros casos se recomienda el uso de corchetes.

**Por ejemplo:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Incorrecto:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Espaciado

* Se tiene que indentar usando 4 espacios, nunca se debe de indentar con tabuladores. Asegúrate de tener activada esta función en Xcode.
* Los métodos o cualquier otro bloque de código que utilice llaves (`if`/`else`/`switch`/`while` etc.) deben de abrirse siempre en la misma línea, pero cerrarse en una línea diferente.

**Por ejemplo:**
```objc
if (user.isHappy) {
    // Hacer algo
}
else {
    // Hacer otra cosa
}
```
* Debe de haber exactamente una línea en blanco entre métodos para mejorar la claridad y organización visual. Los espacios en blanco dentro de los métodos deben de separar funcionalidad, pero deben de ser usados en su mayoría para separar métodos.
* `@synthesize` y `@dynamic` deben declararse en nuevas líneas de código.

## Condicionales

Los condicionales deben de tener siempre llaves, incluso cuando no sean necesarias se deben de utilizar (por ejemplo, cuando sólo hay una línea de código), esto para prevenir [errores](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). Estos errores pueden ser, agregar una segunda línea de código dentro del `if` esperando que sea parte del mismo bloque. Otro [error más peligroso](http://programmers.stackexchange.com/a/16530) 
sería comentar la única línea de código del `if`, lo que convertiría la siguiente línea parte del primer bloque. Además, este estilo es más consistente con los otros condicionales, por lo tanto es mucho más fácil interpretarlo.

**Por ejemplo:**
```objc
if (!error) {
    return success;
}
```

**Incorrecto:**
```objc
if (!error)
    return success;
```

o

```objc
if (!error) return success;
```

### Operador ternario

El operador ternario `?` debe de ser utilizado únicamente cuando ayuda a mejorar la claridad o limpieza del código. Esto es, cuando sólo una condición será evaluada. Evaluar multiples condiciones es mucho más entendible si se hace con un `if`.

**Por ejemplo:**
```objc
result = a > b ? x : y;
```

**Incorrecto:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Manejo de Errores

Cuando los métodos regresen un parámetro error, debe de tratarse el valor regresado y no una variable de error.

**Por ejemplo:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Manejar error
}
```

**Incorrecto:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Manejar error
}
```

Algunas APIs de Apple guardar valores basura en el parámetro de error aunque no exista el error, por lo que manejar una variable de error puede causar falsos negativos en la aplicación (que podría terminar en un fallo).

## Métodos

En la declaración de métodos, debe de existir un espacio después del símbolo `-` o `+`. Deben de existir espacios, igualmente, entre los segmentos del método.

**Por ejemplo:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```