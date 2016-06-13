# NYTimes Objective-C Style Guide

Этот гайд описывает основные правила нописания кода iOS команды в New York Times. Мы будем рады фидбеку в виде [issues](https://github.com/NYTimes/objective-c-style-guide/issues) и [pull request-ам](https://github.com/NYTimes/objective-c-style-guide/pulls). Так же, [мы ищем сотрудников](http://www.nytco.com/careers/).

Спасибо всем [нашим контрибьютором](https://github.com/NYTimes/objective-c-style-guide/graphs/contributors).

## Введение

Тут несколько документов от Apple, которые описывают style guide. Если что-то не упомянуто тут, то оно точно подробно описано в одном из этих документов:
* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

Этот гайд соответствует стандарту IETF [RFC 2119](http://tools.ietf.org/html/rfc2119). Иногда код может не соответствовать этому гайду, но это решение должно быть тщательно обдуманно.

## Содержание

* [Точечная нотация (Dot Notation Syntax)](#dot-notation-syntax)
* [Отступы](#spacing)
* [Условия](#conditionals)
  * [Тернарные операторы](#ternary-operator)
* [Обработка ошибок](#error-handling)
* [Методы](#methods)
* [Переменные](#variables)
* [Именование (Naming)](#naming)
  * [Категории](#categories)
* [Комментарии](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Литералы (Literals)](#literals)
* [Функции CGRect](#cgrect-functions)
* [Константы](#constants)
* [Enumerated Types](#enumerated-types)
* [Bitmasks](#bitmasks)
* [Приватные свойства (Private Properties)](#private-properties)
* [Наименование изображений](#image-naming)
* [Булевы значения](#booleans)
* [Синглтоны](#singletons)
* [Импорты (Imports)](#imports)
* [Протоколы](#protocols)
* [Xcode проект (Project)](#xcode-project)

## Dot Notation Syntax
## Точечная нотация

Точечная нотация РЕКОМЕНДУЕТСЯ вместо скобочной нотации для получения и установки значений свойств.

**Например:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Так делать не надо:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Spacing
## Отступы

* Отступ ДОЛЖЕН задаваться 4-мя пробелами. Никогда не задавайте отступы tab-ами. Убедитесь, что эта настройка указана в Xcode.
* Фигурные скобки метода и другие (`if`/`else`/`switch`/`while` и т.д.) ДОЛЖНЫ открываться на той же строчке, что и оператор. Фигурные скобки ДОЛЖНЫ закрываться на новой строке. 

**Например:**
```objc
if (user.isHappy) {
    // Do something
}
else {
    // Do something else
}
```

* ДОЛЖНА быть одна пустая строка между методами, чтобы поддерживать визуальную ясность и организацию.
* Пробел внутри методов МОЖЕТ разделять функциональность, хотя этот приём часто показывает возможность разделить метод на несколько меньших методов. В методах с длинными и сложными названиями, одна строка пробел МОЖЕТ быть использована, чтобы визуально отделить тело метода.
* Каждый `@synthesize` и `@dynamic` ДОЛЖЕН быть задекларирован на новой строке в реализации.

## Conditionals
## Условия

Conditional bodies MUST use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent [errors](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) can happen where the line “inside” the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**Например:**
```objc
if (!error) {
    return success;
}
```

**Так делать не надо:**
```objc
if (!error)
    return success;
```

or

```objc
if (!error) return success;
```

### Ternary Operator
### Тернарные операторы

The intent of the ternary operator, `?` , is to increase clarity or code neatness. The ternary SHOULD only evaluate a single condition per expression. Evaluating multiple conditions is usually more understandable as an if statement or refactored into named variables.

**Например:**
```objc
result = a > b ? x : y;
```

**Так делать не надо:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Error Handling
## Обработка ошибок

When methods return an error parameter by reference, code MUST switch on the returned value and MUST NOT switch on the error variable.

**Например:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}
```

**Так делать не надо:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Methods
## Методы

In method signatures, there SHOULD be a space after the scope (`-` or `+` symbol). There SHOULD be a space between the method segments.

**Например:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## Variables

Variables SHOULD be named descriptively, with the variable’s name clearly communicating what the variable _is_ and pertinent information a programmer needs to use that value properly.

**Например:**

* `NSString *title`: It is reasonable to assume a “title” is a string.
* `NSString *titleHTML`: This indicates a title that may contain HTML which needs parsing for display. _“HTML” is needed for a programmer to use this variable effectively._
* `NSAttributedString *titleAttributedString`: A title, already formatted for display. _`AttributedString` hints that this value is not just a vanilla title, and adding it could be a reasonable choice depending on context._
* `NSDate *now`: _No further clarification is needed._
* `NSDate *lastModifiedDate`: Simply `lastModified` can be ambiguous; depending on context, one could reasonably assume it is one of a few different types.
* `NSURL *URL` vs. `NSString *URLString`: In situations when a value can reasonably be represented by different classes, it is often useful to disambiguate in the variable’s name.
* `NSString *releaseDateString`: Another example where a value could be represented by another class, and the name can help disambiguate.

Single letter variable names are NOT RECOMMENDED, except as simple counter variables in loops.

Asterisks indicating a type is a pointer MUST be “attached to” the variable name. **For example,** `NSString *text` **not** `NSString* text` or `NSString * text`, except in the case of constants (`NSString * const NYTConstantString`).

Property definitions SHOULD be used in place of naked instance variables whenever possible. Direct instance variable access SHOULD be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information, see [Apple’s docs on using accessor methods in initializer methods and `dealloc`](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Например:**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Так делать не надо:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

#### Variable Qualifiers

When it comes to the variable qualifiers [introduced with ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), the qualifier (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) SHOULD be placed between the asterisks and the variable name, e.g., `NSString * __weak text`. 

## Naming
## Именование

Apple naming conventions SHOULD be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**Например:**

```objc
UIButton *settingsButton;
```

**Not**

```objc
UIButton *setBut;
```

A three letter prefix (e.g., `NYT`) MUST be used for class names and constants, however MAY be omitted for Core Data entity names. Constants MUST be camel-case with all words capitalized and prefixed by the related class name for clarity. A two letter prefix (e.g., `NS`) is [reserved for use by Apple](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/DefiningClasses/DefiningClasses.html#//apple_ref/doc/uid/TP40011210-CH3-SW12).

**Например:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Так делать не надо:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties and local variables MUST be camel-case with the leading word being lowercase.

Instance variables MUST be camel-case with the leading word being lowercase, and MUST be prefixed with an underscore. This is consistent with instance variables synthesized automatically by LLVM. **If LLVM can synthesize the variable automatically, then let it.**

**Например:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Так делать не надо:**

```objc
id varnm;
```

### Categories
### Категории

Categories are RECOMMENDED to concisely segment functionality and should be named to describe that functionality.

**Например:**

```objc
@interface UIViewController (NYTMediaPlaying)
@interface NSString (NSStringEncodingDetection)
```

**Так делать не надо:**

```objc
@interface NYTAdvertisement (private)
@interface NSString (NYTAdditions)
```

Methods and properties added in categories MUST be named with an app- or organization-specific prefix. This avoids unintentionally overriding an existing method, and it reduces the chance of two categories from different libraries adding a method of the same name. (The Objective-C runtime doesn’t specify which method will be called in the latter case, which can lead to unintended effects.)

**Например:**

```objc
@interface NSArray (NYTAccessors)
- (id)nyt_objectOrNilAtIndex:(NSUInteger)index;
@end
```

**Так делать не надо:**

```objc
@interface NSArray (NYTAccessors)
- (id)objectOrNilAtIndex:(NSUInteger)index;
@end
```

## Комментарии

When they are needed, comments SHOULD be used to explain **why** a particular piece of code does something. Any comments that are used MUST be kept up-to-date or deleted.

Block comments are NOT RECOMMENDED, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

## init and dealloc
## init и dealloc

`dealloc` methods SHOULD be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `init` methods SHOULD be placed directly below the `dealloc` methods of any class.

`init` methods should be structured like this:

```objc
- (instancetype)init {
    self = [super init]; // or call the designated initializer
    if (self) {
        // Custom initialization
    }

    return self;
}
```

## Literals
## Литералы

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals SHOULD be used whenever creating immutable instances of those objects. Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**Например:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Так делать не надо:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## `CGRect` Functions
## `CGRect` функции

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, code MUST use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Например:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Так делать не надо:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constants
## Константы

Constants are RECOMMENDED over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants MUST be declared as `static` constants. Constants MAY be declared as `#define` when explicitly being used as a macro.

**Например:**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**Так делать не надо:**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, the new fixed underlying type specification MUST be used; it provides stronger type checking and code completion. The SDK includes a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`.

**Example:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## Bitmasks

When working with bitmasks, the `NS_OPTIONS` macro MUST be used.

**Example:**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
    NYTAdCategoryAutos      = 1 << 0,
    NYTAdCategoryJobs       = 1 << 1,
    NYTAdCategoryRealState  = 1 << 2,
    NYTAdCategoryTechnology = 1 << 3
};
```

## Private Properties
## Приватные свойства

Приватные свойства 
Private properties ДОЛЖНЫ быть декларированы в расширении класса (анонимные категории) в файле реализации (.m) этого класса.

**Например:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Image Naming
## Наименование изображений

Image names should be named consistently to preserve organization and developer sanity. Images SHOULD be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**Например:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` и `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` и `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Изображения, которые используются для одной задачи ДОЛЖНЫ быть сгруппированы в соответствующие группы в папке Images или каталоге Asset'ов.

## Booleans
## Булевы значения

Значения НЕ ДОЛЖНЫ сравниваться конкретно с `YES`, потому что `YES` определяется как `1`, а `BOOL` в Objective-C имеет тип `CHAR`, который имеет длину 8 бит (это значит, что значение `11111110`, вернет `NO`, если будет сравнено с `YES`).

**Для ссылки на объект:**

```objc
if (!someObject) {
}

if (someObject == nil) {
}
```

**Для `BOOL` значения:**

```objc
if (isAwesome)
if (!someNumber.boolValue)
if (someNumber.boolValue == NO)
```

**Так делать не надо:**

```objc
if (isAwesome == YES) // НИКОГДА ТАК НЕ ДЕЛАЙТЕ!
```

Если имя `BOOL` свойства выражено прилагательным, то это имя этого свойства МОЖЕТ содержать `is` префикс, но должно описывать соотствующее имя только для getter'а.

**Например:**

```objc
@property (assign, getter=isEditable) BOOL editable;
```

_Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)._

## Singletons
## Синглтоны

Объекты-синглтоны ДОЛЖНЫ использовать потокобезопасный паттерн для создания их сущностей.
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
Это позволит избежать [возможных и иногда частых крашей](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Imports
## Импорты
Если в файле больше одного выражения import, то они ДОЛЖНЫ быть сгруппированны [вместе](http://ashfurrow.com/blog/structuring-modern-objective-c). Группы МОГУТ быть прокомментированы.

Заметка: Для модулей используйте [@import](http://clang.llvm.org/docs/Modules.html#using-modules) синтакс.

```objc
// Frameworks
@import QuartzCore;

// Models
#import "NYTUser.h"

// Views
#import "NYTButton.h"
#import "NYTUserView.h"
```

## Protocols
## Протоколы

В [протоколах delegate или data source](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/DelegatesandDataSources/DelegatesandDataSources.html), первый параметр каждого метода ДОЛЖЕН быть объектом, отправляющим сообщение.

Это устраняет неоднозначность в ситуациях, когда объект является делегатом многих объектов одного типа, и помогает уточнить намерения для пользователей класса, реализующего методы этого делегата.

**Например:**

```objc
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath;
```

**Так делать не надо:**

```objc
- (void)didSelectTableRowAtIndexPath:(NSIndexPath *)indexPath;
```

## Xcode project
## Xcode проект

Физические файлы ДОЛЖНЫ быть синхронизированы с файлами проекта Xcode, чтобы избежать сумбура. Любая группа в Xcode ДОЛЖНА соответствовать папка в файловой системе. Код ДОЛЖЕН быть сгрупирован не только по типу, но и по функциональности для большей ясности.

Target Build Setting “Treat Warnings as Errors” ДОЛЖНА быть включена. Включите так много [дополнительных warning'ов](http://boredzo.org/blog/archives/2009-11-07/warnings) как только можете. Если нужно игнорировать конкретный warning, используйте [функции Clang pragma](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Другие Objective-C стайл гайды

Если наш не соответствует вашим вкусам, то можете посмотреть другие гайды:
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-style-guide)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
* [Wikimedia](https://www.mediawiki.org/wiki/Wikimedia_Apps/Team/iOS/ObjectiveCStyleGuide)
