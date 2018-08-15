# NYTimes - Guía de estilo de Objective-C

Esta guía de estilo define los líneamientos de desarrollo para los equipos de iOS en The New York Times. Agradecemos sus comentarios en los [issues](https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls) y [tweets](https://twitter.com/nytimesmobile). También, [estamos contratando](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY-10001/73366300/).

Gracias a todos [nuestros contribuyentes](https://github.com/NYTimes/objective-c-style-guide/contributors).

## Introducción

Aquí puedes encontrar algunos de los documentos de Apple sobre las guías de estudio. Si algo no se menciona aquí, probablemente estará cubierto con gran detalle en alguno de éstos:

* [El lenguaje de programación Objective -C](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
* [Guía de fundamentos de Cocoa](https://developer.apple.com/legacy/library/documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
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
  * [Categorías](#categorías)
* [Comentarios](#comentarios)
* [Init y Dealloc](#init-y-dealloc)
* [Literales](#literales)
* [Funciones CGRect](#funciones-cgrect)
* [Constantes](#constantes)
* [Tipos enumerados](#tipos-enumerados)
* [Máscara de bits](#máscara-de-bits)
* [Propiedades privadas](#propiedades-privadas)
* [Nombres de imágenes](#nombres-de-imágenes)
* [Booleanos](#booleano)
* [Singletons](#singletons)
* [Imports](#imports)
* [Protocolos](#protocolos)
* [Proyecto de Xcode](#proyecto-de-xcode)

## Sintaxis Dot Notation

Hay que utilizar punto **siempre** que se acceda o altere alguna propiedad. En todos los otros casos se recomienda el uso de corchetes.

**Por ejemplo:**

```objc
view.backgroundColor = UIColor.orangeColor;
UIApplication.sharedApplication.delegate;
```

**Incorrecto:**

```objc
[view setBackgroundColor:[UIColor orangeColor]];
[UIApplication sharedApplication].delegate;
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

* Debe de haber exactamente una línea en blanco entre métodos para mejorar la claridad y organización visual. 
* Los espacios en blanco dentro de los métodos deben de separar funcionalidad (aunque en algunas ocasiones esto significa que el método puede ser dividido en métodos más pequeños). En métodos con nombres largos que incluyan muchos verbos, una sola línea en blanco puede ser usada para proporcionar una separación visual antes del cuerpo del método.
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

```objc
if (!error) return success;
```

### Operador ternario

El operador ternario `?` debe de ser utilizado únicamente cuando ayuda a mejorar la claridad o limpieza del código. Esto es, cuando sólo una condición será evaluada. Evaluar múltiples condiciones es mucho más entendible si se hace con un `if`.

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

Las variables deben de ser nombradas de la manera más descriptiva posible, con el nombre comunicando de la manera más explicita que _es_ la variable y la información pertinente para que un programador pueda usarla apropiadamente.

* `NSString *title`: Es algo razonable asumir que "title" es un string.
* `NSString *titleHTML`: Esto indica que el título puede incluir HTML por lo que necesita que se parsee para ser mostrado. _El prefijo "HTML" es necesario para que un programador use la variable correctamente._
* `NSAttributedString *titleAttributedString`: Un titulo que ya tiene formato para ser mostrado._En este caso, _`AttributedString` ayuda al programador a tomar una buena decisión dependiendo del contexto._
* `NSDate *now`: _No se necesita más información en el nombre._
* `NSDate *lastModifiedDate`: Sólo usar `lastModified` podría generar ambigüedad; dependiendo del contexto, uno podría asumir distintos tipos con el nombre.
* `NSURL *URL` vs. `NSString *URLString`: En el caso en que un mismo valor pueda ser presentado en diferences clases, es recomendado distinguirlos en el nombre para evitar la confusión que se pueda generar.
* `NSString *releaseDateString`: Otro ejemplo de como una variable puede ser representada por otra clase, por lo que el nombre ayuda a evitar ambigüedades.

Variables con una sola letra como nombre se deben de evitar excepto en los contadores de los ciclos.

Los asteriscos que indican apuntadores pertenecen a la variable, por ejemplo, `NSString *text` no `NSString* text` o `NSString * text`, excepto en los casos de las constantes (`NSString * const NYTConstantString`).

La definición de propiedades debe de ser usada siempre que sea posible, en lugar de la definición de variables de instancia. Las variables directas de instancia deben de ser evitadas, excepto en los métodos de inicialización (`init`, `initWithCoder:`, etc…), los métodos `dealloc` y los métodos de acceso (`set` y `get`). Para más información de el uso de métodos de acceso en inicializadores  y `dealloc` [consulte esta página](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Por ejemplo:**

```objc
@interface NYTSection : NSObject

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

Cuando se trate de la clasificación de variables [agregadas con ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), el clasificador (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) debe de ser puesto entre el asterisco y el nombre de la variable, por ejemplo: `NSString * _weak text`.

## Nombres

Se deben de seguir las convenciones de nombramiento de Apple siempre que sea posible, especialmente las que están relacionadas con las [reglas de manejo de memoria](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Los nombres largos y descriptivos de variables son buenos.

**Por ejemplo:**

```objc
UIButton *settingsButton;
```

**Incorrecto:**

```objc
UIButton *setBut;
```

Un prefijo de tres letras (por ejemplo, `NYT`) siempre se debe de usar para nombres de clases y constantes, pero debe de ser omitido en los nombres de las entidades de Core Data. Las constantes deben de ser 'camel-case' con todas las palabras en mayúsculas y un prefijo relacionado con el nombre de la clase. Usar un prefijo de sólo dos letras (por ejemplo, `NS`) está [reservado para el uso de Apple](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/DefiningClasses/DefiningClasses.html#//apple_ref/doc/uid/TP40011210-CH3-SW12).

**Por ejemplo:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Incorrecto:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Las propiedades y variables locales deben de ser 'camel-case' con la primera palabra en minúsculas.

Las variables de instancia deben de ser 'camel-case' con la primera palabra en minúsculas y debe de tener el prefijo con un guión bajo. Esto es consistente con la sintetización automática de variables de instancia del LLVM. **Si el LLVM puede sintetizar la palabra automáticamente, hay que dejar que lo haga.**

**Por ejemplo:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Incorrecto:**

```objc
id varnm;
```

### Categorías

Las categorías deben de ser usadas para segmentar funcionalidad de manera concisa y deben de ser nombradas para describir esa funcionalidad.

**Por ejemplo:**

```objc
@interface UIViewController (NYTMediaPlaying)
@interface NSString (NSStringEncodingDetection)
```

**Incorrecto:**

```objc
@interface NYTAdvertisement (private)
@interface NSString (NYTAdditions)
```

Los métodos y las propiedades agregadas a una categoría deben ser nombrados con el prefijo de la aplicación u organización. Esto evita que por error se genere un override de algún método existente y además reduce la posibilidad de que dos categorías de diferentes librerías tengan un método con el mismo nombre (en este caso, no se sabe cual de los dos métodos será llamado por lo que puede prestarse a efectos no deseados).

**Por ejemplo:**

```objc
@interface NSArray (NYTAccessors)
- (id)nyt_objectOrNilAtIndex:(NSUInteger)index;
@end
```

**Incorrecto:**

```objc
@interface NSArray (NYTAccessors)
- (id)objectOrNilAtIndex:(NSUInteger)index;
@end
```

## Comentarios

Cuando sean necesarios, los comentarios deben de ser usados para explicar el **porque** de una sección del código hace algo. Los comentarios deben de ser actualizados o eliminados.

Los bloques de comentarios deben de ser evitados, el código debe de ser lo más entendible posible, con la necesidad de pequeñas explicaciones intermitentes en el mismo. Esto no aplica con los comentarios usados para generar documentación.

## init y dealloc

Los métodos `dealloc`deben de ser incluidos al principio de la implementación, justo después de las líneas `@synthesize` y `@dynamic`. El `init` debe de ser puesto debajo de los métodos `dealloc` de cualquier clase.

Los métodos de `init` deben de ser estructurados de la siguiente manera:

```objc
- (instancetype)init {
self = [super init]; // o llamar el inicializador designado
if (self) {
// Inicialización propia
}
return self;
}
```

## Literales

Las literales de `NSString`, `NSArray` y `NSNumber` deben de ser usadas cuando se creen instancias inmutables de esos objetos. Hay que prestar mayor atención a que los valores `nil` no sean pasados a las literales `NSArray` y `NSDictionary`, porque esto ocasionará que la aplicación deje de funcionar.

**Por ejemplo:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Incorrecto:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## Funciones `CGRect`

Cuando se accede a las propiedades `x`, `y`, `width` o `height` de un `CGRect`, siempre se debe de utilizar las funciones de [`CGGeometry` functions](https://developer.apple.com/documentation/coregraphics/cggeometry) en lugar de accederlas directamente. De la referencia de Apple sobre `CGGeometry`:

> Todas las funciones descritas en esta referencia que toman las estructuras de datos de CGRect como entradas implícitamente estandarizan los rectángulos antes de calcular los resultados. Por esta razón, sus aplicaciones deben de evitar leer y escribir directamente la información guardada en la estructura de datos de CGRect. En lugar de eso, se deben de utilizar las funciones descritas aquí para manipular rectángulos y obtener sus características.

**Por ejemplo:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Incorrecto:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constantes

Las constantes tienen preferencia sobre literales o números directo en la línea de código, porque facilitan la reproducción de variables usadas en la aplicación y pueden ser cambiadas fácilmente sin la necesidad de buscar y reemplazar. Las constantes deben de ser declaradas como `static` y no `#define` excepto cuando se esté usando explícitamente como macro.

**Por ejemplo:**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**Incorrecto:**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## Tipos enumerados

Cuando se estén utilizando `enum` se recomienda utilizar la nueva especificación de tipo fijo subyacente, ya que tiene una forma más fuerte de verificación de código. El SDK incluye una macro que facilita el uso de tipos fijos subyacentes: `NS_ENUM()`.

**Ejemplo:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## Máscara de bits

Cuando se trabaje con máscara de bits, se debe de usar el macro `NS_OPTIONS`.

**Ejemplo:**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
    NYTAdCategoryAutos      = 1 << 0,
    NYTAdCategoryJobs       = 1 << 1,
    NYTAdCategoryRealState  = 1 << 2,
    NYTAdCategoryTechnology = 1 << 3
};
```

## Propiedades privadas

Las propiedades privadas deben de ser declaradas en las extensiones de las clases (categorías anónimas) en el archivo de implementación de una clase.

**Ejemplo:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Nombres de imágenes

Los nombres de imágenes deben de preservar la organización de manera consistente y ayudar al desarrollador. Los nombres deben de utilizar 'camel-case' y describir de manera explicita su proposito, seguido por el nombre de la clase o propiedad a la que serán asignados sin prefijo (en caso de que exista una), seguido de una descripción de color y/o colocación, y por último el estado al que corresponde.

**Ejemplo:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Las imágenes que se utilicen para propositos similares deben de estar agrupadas respectivamente en la carpeta de Imágenes o el catálogo de 'Assets'.

## Booleanos

Nunca se debe de comparar algo directamente con `YES`, porque `YES` está definido como un `1`, y un `BOOL` en Objective-C es de tipo `CHAR` de 8 bits (por lo que el valor de `1111110` regresaría un `NO` si es comparado con `YES`).

**Para el apuntador de un objeto:**

```objc
if (!someObject) {
}

if (someObject == nil) {
}
```

**Para un valor `BOOL`:**

```objc
if (isAwesome)
if (!someNumber.boolValue)
if (someNumber.boolValue == NO)
```

**Incorrecto:**

```objc
if (isAwesome == YES) // Nunca hacer esto.
```

Si el nombre de una propiedad `BOOL` está expresado como un adjetivo la propiedad puede omitir el prefijo `is` pero debe de especificar el nombre convencional para el acceso (‘getter’).

**Por ejemplo:**

```objc
@property (assign, getter=isEditable) BOOL editable;
```

_Texto y ejemplo obtenido de [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)._

## Singletons

Los objetos Singletons deben de usar un patrón 'thread-safe' para la creación de una instancia compartida.

```objc
+ (instancetype)sharedInstance {
    static id sharedInstance = nil;

    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedInstance = [[[self class] alloc] init];
    });

    return sharedInstance;
}
```

Esto ayudará a prevenir [que la aplicación deje de funcionar de manera ocasional o frecuente](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Imports

Si hay más de una declaración de 'import', se deben de agrupar las declaraciones [juntas](http://ashfurrow.com/blog/structuring-modern-objective-c). Comentar cada grupo es opcional.

Nota: Para los modulos usar la sintaxis [@import](http://clang.llvm.org/docs/Modules.html#using-modules).

**Por ejemplo:**

```objc
// Frameworks
@import QuartzCore;

// Modelos
#import "NYTUser.h"

// Vistas
#import "NYTButton.h"
#import "NYTUserView.h"
```

## Protocolos

En un [protocolo de delegado o 'data source'](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/DelegatesandDataSources/DelegatesandDataSources.html), el primer parámetro de cada método debe de ser el objeto que envía el mensaje.

Esto ayuda a evitar ambigüedades en los casos en que un objeto es delegado de múltiples objetos, además ayuda a clarificar la lectura de la clase que implementa los métodos del delegado.

**Por ejemplo:**

```objc
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath;
```

**Incorrecto:**

```objc
- (void)didSelectTableRowAtIndexPath:(NSIndexPath *)indexPath;
```

## Proyecto de Xcode

Los archivos físicos deben de asemejar al proyecto de Xcode para evitar el desorden de los archivos. Cualquier grupo que se haga en Xcode debe de ser reflejado en las carpetas del proyecto. El código debe de ser agrupado no sólo por su tipo, si no además por su funcionalidad para mayor claridad.

Cuando sea posible, siempre hay que activar el "Treat Warnings as Errors" (tratar alertas como errores) en los 'Build Settings' del objetivo y activar la mayor cantidad de [alertas adicionales](http://boredzo.org/blog/archives/2009-11-07/warnings) como sea posible. Si es necesario ignorar ciertas alertas, se puede utilizar la [funcionalidad Clang’s pragma](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

## Otras guías de estilo de Objective-C

Si nuestra guía de estilo no es lo que estás buscando, puedes encontrar otras guías de estilo aquí:

* [Google](https://google.github.io/styleguide/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
* [Wikimedia](https://www.mediawiki.org/wiki/Wikimedia_Apps/Team/iOS/ObjectiveCStyleGuide)
