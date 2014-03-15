# NYTimes • Objective-C Stilanleitung

Diese Anleitung fasst die Coding Standards des iOS Teams der New York Times zusammen. Wir freuen uns über Feedback unter [issues] (https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls) und [tweets](https://twitter.com/nytimesmobile). [Wir suchen aktuell noch Verstärkung](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY/2572221/).

Danke an alle [Beitragenden](https://github.com/NYTimes/objective-c-style-guide/contributors).

## Einleitung

Hier sind einige Dokumente von Apple, durch die die Anleitung geprägt wurde. Wenn hier etwas fehlen sollte, wird es wahrscheinlich in einer der folgenden Quellen detailliert betrachtet:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Punktnotation](#dot-notation-syntax)
* [Abstände](#spacing)
* [Bedingungen](#conditionals)
  * [Ternärer (dreifacher) Operator](#ternary-operator)
* [Fehlerbehandlung](#error-handling)
* [Methoden](#methods)
* [Variablen](#variables)
* [Bezeichnung](#naming)
* [Kommentare](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Literale](#literals)
* [CGRect Funktionen](#cgrect-functions)
* [Konstanten](#constants)
* [Enumarationen](#enumerated-types)
* [Private Properties](#private-properties)
* [Bezeichnung von Bildern](#image-naming)
* [Wahrheitswerte](#booleans)
* [Singletons (Entwurfsmuster)](#singletons)
* [Xcode Projekt](#xcode-project)

## Punktnotation

Die Punktnotation sollte **immer** verwendet werden, um auf Properties zuzugreifen oder sie zu verändern. Die Notation mit eckigen Klammern wird in allen anderen Fällen bevorzugt. 

**Beispiel:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```
**Nicht:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Abstände

* Einrückungen benutzen 4 Leerzeichen. Rücke niemals mit Tabs ein. Versichere dich, die entsprechende Einstellung in Xcode vorzunehmen.
* Bei Methoden und in anderen Fällen (`if`/`else`/`switch`/`while` etc.), wird die öffnende Klammer immer in die gleiche Zeile entsprechend der tatsächlichen Anweisung - die schließende Klammer jedoch in eine neue Zeile, gesetzt. 

**Beispiel:**
```objc
if (user.isHappy) {
//Do something
}
else {
//Do something else
}
```

* Es soll genau eine leere Zeile zwischen den Methoden eingefügt werden, um Übersicht und Organisation zu fördern. Leerzeichen innerhalb von Methoden, sollte funktional unterteilt  sein, wobei in vielen Fällen neue Methoden verwendet werden sollten.

* `@synthesize` und `@dynamic` sollten in der Implementation jeweils in neuen Zeilen deklariert werden.

## Bedingungen

Der Rumpf einer Bedingung sollte immer Klammern verwenden, auch wenn es ohne Klammern stehen könnte (es ist eh nur eine Zeile), damit [Fehler](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256) vermieden werden.

Diese Fehler entstehen beim Einfügen einer zweiten Zeile, von der man erwartet, dass sie Teil der  if-Anweisung wird.
Ein weiterer, [sogar viel gefährlicherer Fehler](http://programmers.stackexchange.com/a/16530) könnte passieren, wenn die Zeile "innerhalb" der if-Anweisung auskommentiert wird und die nächste Zeile unwillentlich Teil dieser if-Anweisung wird. Davon abgesehen, ist diese Form viel konsistenter, verglichen mit den anderen Bedingungsanweisungen und deshalb auch einfacher zu lesen.

**Beispiel:**
```objc
if (!error) {
    return success;
}
```

**Nicht:**
```objc
if (!error)
    return success;
```

oder

```objc
if (!error) return success;
```

### Ternärer (dreifacher) Operator

Der ternäre Operator, ? , sollte nur verwendet werden, wenn es der Klarheit dient und den Code sauber aussehen lässt. Eine einzelne Bedingung ist normalerweise alles, was ausgewertet werden sollte. Mehrere Bedingungen sollten besser in einer if-Anweisung ausgewertet werden oder in Instanzvariablen überarbeitet werden.

**For example:**
```objc
result = a > b ? x : y;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Fehlerbehandlung

Wenn Methoden einen Fehler als Parameter in Form einer Referenz zurückgeben, sollte der Rückgabewert der Methode überprüft werden, nicht die Fehler-Variable.

**Beispiel:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}
```

**Nicht:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Einige der Apple API's schreiben im erfolgreichen Fall Müll in die Parameter (wenn nicht NULL), so dass das Auswerten eines Fehlers falsche Ergebnisse bringen kann (und folglich das Programm abstürzen könnte).

## Methoden

In Methoden Signaturen sollte Abstand hinter dem +/- Zeichen sein. Es sollte ein Leerzeichen zwischen den einzelnen Teilen der Methodensegmente eingefügt werden.

**Beispiel**:
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## Variablen

Variablen sollten so deskriptiv wie möglich sein. Variablen mit einzelnen Buchstaben sollten vermieden werden, wenn sie nicht in `for()` Schleifen verwendet werden.

Asterisks, die auf Zeiger hinweisen, gehören zur Variable - z.B. 	`NSString *text` nicht `NSString* text	` oder `NSString * text`. Ausnahmen bilden die Konstanten.

Property Eigenschaften sollten, wenn immer es möglich ist, an Stelle von einfachen Instanzvariablen verwendet werden. Direkter Zugriff auf Instanzvariablen sollte vermieden werden, Ausnahmen bilden Methoden zur Initialisierung (`init	`, `initWithCoder:`, etc...), `dealloc` Methoden und innerhalb von eigenen Zugriffsmethoden (Setter/Getter). Weitere Informationen zur Verwendung von Zugriffsmethoden und Methoden zur Initialisierung und dealloc, finden sich [hier](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)

**Beispiel:**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Nicht:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

## Benennung

Die von Apple vorgegebenen Konventionen zur Benennung sollten wenn immer möglich eingehalten werden. Besonders jene zur [Speicherverwaltung](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)([NARC](http://stackoverflow.com/a/2865194/340508)).

Lange, ausführliche Methoden- und Variablennamen sind gut.

**Beispiel:**

```objc
UIButton *settingsButton;
```

**Nicht**

```objc
UIButton *setBut;
```

Ein Prefix aus drei Buchstaben (z.B. `NYT`) sollte immer vor Klassennamen und Konstanten stehen, kann bei Core Data Entitäten aber weggelassen werden. Konstanten sollten "Camel-Case" sein, alle Wörter beginnen mit Großbuchstaben und der entsprechende Klassenname wird vorangestellt, um für Klarheit zu sorgen.

**Beispiel:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Nicht:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties und lokale Variablen sollten ebenfalls "Camel-Case" sein, wobei der erste Buchstabe dann klein geschrieben wird.

Instanzvariablen sollten "Camel-Case" sein, wobei das erste Wort klein geschrieben wird und ein Unterstrich vorangestellt wird. Das ist einheitlich mit der Art und Weise, wie LLVM Instanzvariablen automatisch synthetisiert **Wenn LLVM die Variablen automatisch synthetisiert, sollte man das auch zulassen.**

**Beispiel:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**

```objc
id varnm;
```

## Kommentare

Wenn sie gebraucht werden, sollten Kommentare verwendet werden um zu erklären, **warum** ein bestimmtes Codefragment etwas tut.   Alle Kommentare die verwendet werden, müssen ständig aktuell sein oder andernfalls gelöscht werden. 

Blockkommentare sollten generell vermieden werden, da Code (abgesehen von ein paar erklärenden Zeilen zwischendurch) so selbsterklärend wie möglich sein sollte. Das bezieht sich nicht auf Kommentare, die angewendet werden, um eine Dokumentation zu erstellen.

## init und dealloc

`dealloc` Methoden sollten direkt an den Anfang der Implementation, direkt hinter de `@synthesize` und `@dynamic` Aufruf platziert werden.

`init` Methoden sollten folgendermaßen strukturiert werden:

```objc
- (instancetype)init {
    self = [super init]; // oder Aufruf des designated initalizer
    if (self) {
        // Custom initialization
    }

    return self;
}
```

## Literale

`NSString`, `NSDictionary`, `NSArray` und `NSNumber` Literale sollten immer verwendet werden, wenn nicht veränderbare (immutable) Instanzen dieser Klassen erstellt werden. Besonders sollte darauf geachtet werden keine `nil` Werte in die `NSArray` und `NSDictionary` Literale geschrieben werden, da diese einen Absturz verursachen werden.

**Beispiel:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Nicht:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## CGRect Funktionen

Beim Zugriff auf `x`, `y`, `width` oder `height` eines CGRect sollten statt des direkt Zugriffs auf die `struct` Werte, immer die [`CGGeometry` Funktionen](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) verwendet werden.

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Beispiel:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Nicht:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Konstanten

Konstanten werden gegenüber in-line Zeichenketten oder Zahlen bevorzugt, da sie eine einfache Wiederverwendung von gemeinsam verwendeten Variablen erlauben. Zudem können sie einfach ersetzt werden, ohne sie zuvor mit Finde und Ersetze ausfindig gemacht zu haben. Konstanten sollten als `static` Konstanten ud nicht mit #define deklariert werden, so fern sie nicht explizit in Makros verwendet werden.

**Beispiel:**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**Nicht:**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## Aufzählungstypen

Bei der Verwendung von Enumerierungen wird empfohlen, von der neueren Typ-Spezifizierung Gebrauch zu machen, da eine stärkere Typ-Prüfung und Code-Vervollständigung mit inbegriffen ist. Das SDK stellt nun ein Makro mit dem festgelegten Typen `NS_ENUM` zur Verfügung.

**Beispiel:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## Private Eigenschaften

Private Eigenschaften sollten in den Klassenerweiterungen der Implementierung einer Klasse deklariert werden (Anonyme Kategorien). 

**Beispiel:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Benennung von Bildern

Bilder sollten einheitlich benannt werden um der Verwirrung bei den Entwicklern und im Unternehmen vorzubeugen. Sie sollten als eine Camel-Case Zeichenkette benannt werden und ihr Name sollte auf ihre Verwendung aufmerksam machen. Darauf folgt der Name (ohne Präfix) der Klasse oder der Eigenschaft, die sie anpassen (wenn sie das überhaupt tun). Darauf folgt eine weitere Beschreibung ihrer Farbe, Position und schliesslich der Zustand.

**Beispiel:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Bilder die in einem ähnlichen Bereich eingesetzt werden, sollten innerhalb des Bilder-Ordners in entsprechende Unterordner eingeteilt werden.

## Wahrheitswerte (Booleans)

Da `nil` zu `NO` aufgelöst wird, ist es nicht notwendig, es in Bedingungen zu vergleichen. Vergleiche nie etwas direkt mit `YES`, da `YES` als 1 definiert ist und ein `BOOL`-Typ kann 8 Bits lang sein.

Dadurch ist eine Gleichmäßigkeit über mehrere Files hinweg und bessere visuelle Klarheit gewährleistet.

**Beispiel:**

```objc
if (!someObject) {
}
```

**Not:**

```objc
if (someObject == nil) {
}
```

-----

**Für ein `BOOL`, hier ein weiteres Beispiel:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**Not:**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Niemals so.
```

-----

Wenn der Name eines `BOOL` Property als Adjektiv formuliert wurde, kann die Vorsilbbe "is" weggelassen werden. Allerdings sollte der konventionelle Name für den Getter Zugriff spezifiert werden, z.B.:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text und Beispiel stammen von [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Singletons sollten ein thread-sicheres Muster bei der Erstellung ihrer Instanzen einhalten.

```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```

Dies wird [einigen möglichen Abstürzen] vorbeugen (http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Xcode Projekt

Die real vorhanden Dateien sollten mit den Dateien in Xcode synchronisiert werden, um ein Durcheinander zu vermeiden. Alle Gruppen, die in Xcode erstellt wurden, sollten sich im Dateisystem widerspiegeln. Code sollte nicht nur nach seinem Typ, sondern auch nach den entsprechenden Features gruppiert werden, um eine bessere Übersicht zu gewährleisten.

Wenn möglich, sollte die Option "Treat Warnings as Errors" in den Target Build Einstellungen angepasst werden und soviel [zusätzliche Warnungen] wie möglich ausgeben (http://boredzo.org/blog/archives/2009-11-07/warnings). Verwende [Clangs pragma feature] (http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas), wenn eine bestimmte Warnung ignoriert werden soll.

# Weitere Objective-C Styleguides

Wenn unsere Version nicht Euren Geschmack trifft, schaut euch ein paar andere Styleguides an:

* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
