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

## Точечная нотация (Dot Notation Syntax)

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

## Отступы (Spacing)

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

## Условия (Conditionals)

Тело условия ДОЛЖНО использовать фигурные скобки всегда, даже в ситуации, когда тело может записано в одну строку, чтобы избежать [ошибок](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). Эти ошибки включают добавление второй строки и ожидание того, что оно будет частью этого if-выражения. Другой, [гораздо более опасный дефект](http://programmers.stackexchange.com/a/16530) может проявиться, если строка “внутри” if-выражения будет закомментирована, а следующая строка неожиданно станет частью этого if-выражения. В дополнение, этот стиль гораздо проще сочетается с другими условиями, а следовательно – его легче понимать.

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

### Тернарные операторы (Ternary Operator)

Назначение тернарного оператора, `?` , повысить ясность или опрятность кода. Тернарный оператор ДОЛЖЕН использоваться только для одного условия в выражении. Использование для нескольких условий обычно более понимаемо, если используется несколько if-выражений или именованных переменных с условиями.

**Например:**
```objc
result = a > b ? x : y;
```

**Так делать не надо:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Обработка ошибок (Error Handling)

Когда метод возвращает параметр с ошибкой по ссылке, код ДОЛЖЕН переключаться на основе возвращенного значения и НЕ ДОЛЖЕН переключаться на основе переменной ошибки.

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

Некоторые API от Apple записывают мусорные значения в параметр ошибки (if non-NULL) в случае успешного выполнения, поэтому переключение по переменной ошибки может вызывать ошибочный негативный результат (и, возможно, краш).

## Методы (Methods)

В подписи метода ДОЛЖЕН быть пробел после символа сферы применения метода (`-` или `+`). ДОЛЖЕН быть пробел между сегментами метода.

**Например:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## Переменные (Variables)

Переменные ДОЛЖНЫ называться понятно. Имя переменной ДОЛЖНО показывать, чем _является_ эта переменная, и содержать уместную информацию, чтобы программист мог использовать её правильно. 

**Например:**

* `NSString *title`: Разумно предположить, что title является строкой
* `NSString *titleHTML`: Эта переменная показывает, что содержит HTML, который необходимо обработать, прежде чем выводить. _“HTML” необходимо программисту, чтобы использовать переменную и её значение эффективно._
* `NSAttributedString *titleAttributedString`: title, который уже отформатирован для отображения. _`AttributedString` подсказывает, что это значение не просто заголовок. Добавление такой подсказки – это разумный выбор на основе контекста._
* `NSDate *now`: _Не требуется дополнительных объяснений._
* `NSDate *lastModifiedDate`: Просто `lastModified` может быть двусмысленным; на основе контекста можно предположить, что эта переменная относится к разным типам.
* `NSURL *URL` vs. `NSString *URLString`: В ситуациях, когда значение может быть представленно разными классами, стоит указать класс в имени переменной.
* `NSString *releaseDateString`: Другой пример, где значение может быть представленно другим классом, а имя переменной помогает разобраться.

Переменные с одной буквой в названии НЕ РЕКОМЕНДУЮТСЯ, за исключением счетчиков в циклах.

Звездочка, показывающая тип ссылки, ДОЛЖНА быть “прикреплена” к имени переменной. **Например,** `NSString *text` **, а не** `NSString* text` или `NSString * text`, за исключением констант (`NSString * const NYTConstantString`).

Определения свойств ДОЛЖНЫ использоваться вместо "голых" переменных объекта везде, где это возможно. Напрямую использование переменных объекта НУЖНО избегать везде, кроме методов инициализации (`init`, `initWithCoder:`, etc…), `dealloc` методов и внутри собственных setter'ов и getter'ов. Больше подробностей можно найти в [Apple’s docs on using accessor methods in initializer methods and `dealloc`](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

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

#### Спецификаторы переменных (Variable Qualifiers)

Когда мы говорит про спецификаторы переменных, [представленные вместе с ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), спецификатор (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) ДОЛЖЕН быть расположен между звездочкой и именем переменной: `NSString * __weak text`. 

## Именование (Naming)

Правила именования Apple ДОЛЖНЫ соблюдаться везде, где это возможно, особенно когда мы говорим про [правила управления памятью](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Длинные и подробные названия методов и переменных – хорошие. 

**Например:**

```objc
UIButton *settingsButton;
```

**Так делать не надо:**

```objc
UIButton *setBut;
```

Трехбуквенный префикс (например, `NYT`) ДОЛЖЕН быть использовать для имён классов и констант, но МОГУТ быть опущены для имён сущностей Core Data. Константы ДОЛЖНЫ использовать верблюжью нотацию, где все слова идут с большой буквы и имеют префикс равный имени связанного класса для большей понятность. Двухбуквенный префикс (например, `NS`) [зарезервирован Apple](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/DefiningClasses/DefiningClasses.html#//apple_ref/doc/uid/TP40011210-CH3-SW12).

**Например:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Так делать не надо:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Свойства и локальный переменные ДОЛЖНЫ использовать верблюжью нотацию, где первое слово с маленькой буквы.

Переменные экземпляра ДОЛЖНЫ быть в верблюжей нотации, где первое слово с маленькой буквы и ИМЕЕТ префикс подчеркивание. Это согласуется с переменными экземпляра, синтезированными автоматически LLVM. **Если LLVM может синтезировать переменную автоматически, дайте ему это сделать.**

**Например:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Так делать не надо:**

```objc
id varnm;
```

### Категории (Categories)

Категории РЕКОМЕНДОВАНЫ для краткой части функциональности и должны называться так, чтобы была понятна эта функциональность.

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

Методы и свойства, добавленные в категориях, ДОЛЖНЫ называться с префиксом приложения или организации. Это позволит избежать случайного переопределения существующих методов и снизит вероятность появления двух категорий из разных библиотек, добавляющих метод с одинаковым именем. (The Objective-C runtime не декларирует, какой метод будет использован в такой ситуации, что может привести к неприятным ситуациям.)

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

Когда они нужны, комментарии ДОЛЖНЫ использоваться, чтобы объяснить **ЗАЧЕМ** в конкретном кусочке кода делается что-то. Любой комментарий, который используется, ДОЛЖЕН поддерживаться актуальным или быть удалён.

Блоки комментариев НЕ РЕКОМЕНДУЮТСЯ, так как код должен быть само-документируемым настолько, насколько это возможно. Должна быть потребность только в коротких объяснениях нескольких строчек. Это не относится к комментариям, используемым для создания документации.

## init и dealloc

`dealloc` метод ДОЛЖЕН быть расположен в верхней части реализации (`.m` файл), сразу после `@synthesize` и `@dynamic`. `init` метод ДОЛЖЕН быть расположен сразу после `dealloc` метода в любом классе.

`init` методы должны иметь подобную структуру:

```objc
- (instancetype)init {
    self = [super init]; // or call the designated initializer
    if (self) {
        // Custom initialization
    }

    return self;
}
```

## Литералы (Literals)

`NSString`, `NSDictionary`, `NSArray`, и `NSNumber` литералы ДОЛЖНЫ быть использованы при создании нередактируемых (immutable) объектов. Уделите особое внимание тому, что `nil` значения не принимаются `NSArray` и `NSDictionary` литералами, так как это вызовет краш.

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

## `CGRect` функции

Когда используется `x`, `y`, `width`, или `height` сущности `CGRect`, код ДОЛЖЕН использовать [`CGGeometry` функции](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) вместо прямого доступа к части структуры. Из описания `CGGeometry` от Apple:
> Все функции, описанные в этом документе, которые получают данные из структур CGRect, неявно стандартизируют эти прямоугольники перед подсчетом их результатов. По этой причине, ваши приложения должны избегать непосредственно чтения и записи данных, хранящихся в структуре `CGRect`. Вместо этого следует использовать функции, описанные здесь, чтобы манипулировать прямоугольниками и получать их характеристики.

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

## Константы

Константы РЕКОМЕНДУЮТСЯ вместо применения литералов или чисел внутри кода, так как они позволяют обеспечить легкий доступ к популярным переменным и могут быть быстро изменены без необходимости поиска и замены. Константы должны быть объявлены как `static`. Константы МОГУТ быть объявлены как `#define`, если они явно используются в качестве макроса.

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

## Enum-типы (Enumerated Types)

Когда используете `enum`ы, новая фиксированная спецификация ДОЛЖНА использоваться; она предоставляет более четкую проверку типа и автодополнение. SDK включает макрос, чтобы использовать этот фиксированный базовый тип: `NS_ENUM()`.

**Пример:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## Битовые маски (Bitmasks)

Когда работаете с битовыми масками, макрос `NS_OPTIONS` ДОЛЖЕН использоваться. 

**Например:**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
    NYTAdCategoryAutos      = 1 << 0,
    NYTAdCategoryJobs       = 1 << 1,
    NYTAdCategoryRealState  = 1 << 2,
    NYTAdCategoryTechnology = 1 << 3
};
```

## Приватные свойства (Private Properties)

Приватные свойства 
Приватные свойства ДОЛЖНЫ быть декларированы в расширении класса (анонимные категории) в файле реализации (.m) этого класса.

**Например:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Наименование изображений

Имена изображений должны называться последовательно, чтобы сохранить организацию и здравомыслие разработчиков. Изображения ДОЛЖНЫ быть называться с использованием одной строки в верблюжьей нотации с описание их назначения, после этого должен следовать имя класса или свойства, которое они изменяют, после – описание цвета / места, и в конце – состояние. 

**Например:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` и `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` и `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Изображения, которые используются для одной задачи ДОЛЖНЫ быть сгруппированы в соответствующие группы в папке Images или каталоге Asset'ов.

## Булевы значения (Booleans)

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

_Текст и пример позаимстован у [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)._

## Синглтоны (Singletons)

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

## Импорты (Imports)
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

## Протоколы (Protocols)

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
