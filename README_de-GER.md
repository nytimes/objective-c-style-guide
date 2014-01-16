# NYTimes • Objective-C Stilanleitung

Diese Anleitung fasst die Coding Standards des iOS Teams der New York Times zusammen. Wir freuen uns über Feedback unter [issues] (https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls) und [tweets](https://twitter.com/nytimesmobile). [Wir suchen aktuell nach Verstärkung](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY/2572221/).

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
* Bei Methoden und in anderen Fällen (`if`/`else`/`switch`/`while` etc.), wird die öffnende Klammer immer auf die gleiche Zeile wie die eigentliche Anweisung, die schließende Klammer jedoch in eine neue Zeile, gesetzt. 

**Beispiel:**
```objc
if (user.isHappy) {
//Do something
}
else {
//Do something else
}
```

* Es soll genau eine leere Zeile zwischen den Methoden eingefügt werden, um optische Klarheit und Organisation zu unterstützen. Leerzeichen innerhalb von Methoden, sollte funktional unterteilt  sein, wobei in vielen Fällen neue Methoden verwendet werden sollten.

* `@synthesize` und `@dynamic` sollten in der Implementation jeweils in neuen Zeilen deklariert werden.

## Bedingungen

Der Rumpf einer Bedingung sollte immer Klammern verwenden, auch wenn es ohne Klammern stehen könnte (es ist nur eine Zeile), damit [Fehler](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256) vermieden werden.

Diese Fehler entstehen beim Einfügen einer zweiten Zeile, von der man erwartet, dass sie Teil der  if-Anweisung wird.
Ein weiterer, [sogar viel gefährlicherer Fehler](http://programmers.stackexchange.com/a/16530) könnte passieren, wenn die Zeile "innerhalb" der if-Anweisung auskommentiert wird und die nächste Zeile unwillentlich Teil dieser if-Anweisung wird. Davon abgesehen, ist diese Form viel gleichmäßiger, verglichen mit den anderen Bedingungsanweisungen und deshalb auch einfacher zu lesen.

**For example:**
```objc
if (!error) {
    return success;
}
```

**Not:**
```objc
if (!error)
    return success;
```

or

```objc
if (!error) return success;
```
