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

* [Punktnotation Syntax](#dot-notation-syntax)
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
