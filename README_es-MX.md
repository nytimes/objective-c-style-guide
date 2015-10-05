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
* [Métodos](#métodos)
* [Variables](#variables)
* [Nombres](#nombres)
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

## Variables

Las variables deben de ser nombradas de la manera más descriptiva posible. Variables con una sola letra como nombre se deben de evitar excepto en los contadores de los ciclos `for`.

Los asteriscos que indican apuntadores pertenecen a la variable, por ejemplo, `NSString *text` no `NSString* text` o `NSString * text`, excepto en los casos de las constantes (`NSString * const NYTConstantString`).

La definición de propiedades debe de ser usada siempre que sea posible, en lugar de la definición de variables de instancia. Las variables directas de instancia deben de ser evitadas, excepto en los métodos de inicialización (`init`, `initWithCoder:`, etc…), los métodos `dealloc` y los métodos de acceso (`set`y `get`). Para más información de el uso de métodos de acceso en inicializadores  y `dealloc` consulte esta página](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Por ejemplo:**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Incorrecto:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```
### Clasificación de variables

Cuando se trate de la clasificación de variables [agregadas con ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), el clasificador (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) debe de ser puesto entre el asterisco y el nombre de la variable, por ejemplo: `NSString * _wak text`.

## Nombres

Se deben de seguir las convenciones de nombramiento de Apple siempre que sea posible, especialmente las que están relacionadas con las [reglas de manejo de memoria] (https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Los nombres largos y descriptivos de variables son buenos.

**Por ejemplo:**

```objc
UIButton *settingsButton;
```

**Incorrecto:**

```objc
UIButton *setBut;
```

Un prefijo de tres letras (por ejemplo, `NYT`) siempre se debe de usar para nombres de clases y constantes, pero debe de ser omitido en los nombres de las entidades de Core Data. Las constantes deben de ser camel-case con todas las palabras en mayúsculas y un prefijo relacionado con el nombre de la clase. Usar un prefijo de sólo dos letras (por ejemplo, `NS`) está [reservado para el uso de Apple](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/DefiningClasses/DefiningClasses.html#//apple_ref/doc/uid/TP40011210-CH3-SW12).

**Por ejemplo:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Incorrecto:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Las propiedades y variables locales deben de ser camel-case con la primera palabra en minúsculas.

Las variables de instancia deben de ser camel-case con la primera palabra en minúsculas y debe de tener el prefijo con un guión bajo. Esto es consistente con la sintetización automática de variables de instancia del LLVM. **Si el LLVM puede sintetizar la palabra automáticamente, hay que dejar que lo haga.**

**Por ejemplo:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Incorrecto:**

```objc
id varnm;
```
