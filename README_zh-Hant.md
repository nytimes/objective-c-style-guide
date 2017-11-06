# 紐約時報 Objective-C 規範指南

這份規範指南定義了紐約時報的iOS團隊的程式碼寫作規範

## 介紹

以下是Apple官方的程式碼寫作指南文件，如果這邊沒有提到的話，應該就在以下的文件裡可以找到：

* [Objective-C 程式語言][Introduction_1]
* [Cocoa 基本原理指南][Introduction_2]
* [Cocoa 程式碼指南][Introduction_3]
* [iOS App 程式撰寫指南][Introduction_4]

[Introduction_1]:http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html

[Introduction_2]:https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html

[Introduction_3]:https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html

[Introduction_4]:http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html


## 目录

* [點語法](#點語法)
* [空格間距](#空格間距)
* [條件判斷](#條件判斷)
* [三元運算子](#三元運算子)
* [錯誤處理](#錯誤處理)
* [方法](#方法)
* [變數](#變數)
* [命名](#命名)
* [註解](#註解)
* [Init 和 Dealloc](#init-和-dealloc)
* [字面量](#字面量)
* [CGRect 函數](#CGRect-函數)
* [常數](#常數)
* [列舉型別](#列舉型別)
* [位掩码](#位掩码)
* [私有屬性](#私有屬性)
* [圖片命名](#圖片命名)
* [布林值](#布林值)
* [單例](#單例)
* [導入](#導入)
* [Xcode 工程](#Xcode-工程)

## 點語法

應該 **始終**使用點語法來訪問或者修改屬性，使用其他實體首選中括號。

**推薦：**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**不推薦：**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## 空格間距

* 一個縮行使用4個空格，永遠不要使用tab來進行縮排。請確保在Xcode中設定此偏好。
* 方法的大括號和其他的大括號（`if`/`else`/`switch`/`while` 等等）應該與宣告處位於同一行，但在新的一行結束

**推荐：**
```objc
if (user.isHappy) {
    // Do something
}
else {
    // Do something else
}
```
* 方法之間應該正好空一行，有助於視覺清晰度和程式碼的組織性。在方法中的功能區域之間應該使用空白分開，但往往可能應該創建一個新的方法。
* `@synthesize` 和 `@dynamic` 在使用上每個應該都佔一個新行。


## 条件判断

条件判断主体部分应该始终使用大括号括住来防止[出错][Condiationals_1]，即使它可以不用大括号（例如它只需要一行）。这些错误包括添加第二行（代码）并希望它是 if 语句的一部分时。还有另外一种[更危险的][Condiationals_2]，当 if 语句里面的一行被注释掉，下一行就会在不经意间成为了这个 if 语句的一部分。此外，这种风格也更符合所有其他的条件判断，因此也更容易检查。

**推荐：**
```objc
if (!error) {
    return success;
}
```

**反对：**
```objc
if (!error)
    return success;
```

或

```objc
if (!error) return success;
```


[Condiationals_1]:(https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256)
[Condiationals_2]:http://programmers.stackexchange.com/a/16530

### 三目运算符

三目运算符，? ，只有当它可以增加代码清晰度或整洁时才使用。单一的条件都应该优先考虑使用。多条件时通常使用 if 语句会更易懂，或者重构为实例变量。

**推荐：**
```objc
result = a > b ? x : y;
```

**反对：**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## 错误处理

当引用一个返回错误参数（error parameter）的方法时，应该针对返回值，而非错误变量。

**推荐：**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // 处理错误
}
```

**反对：**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // 处理错误
}
```
一些苹果的 API 在成功的情况下会写一些垃圾值给错误参数（如果非空），所以针对错误变量可能会造成虚假结果（以及接下来的崩溃）。

## 方法

在方法签名中，在 -/+ 符号后应该有一个空格。方法片段之间也应该有一个空格。

**推荐：**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## 变量

变量名应该尽可能命名为描述性的。除了 `for()` 循环外，其他情况都应该避免使用单字母的变量名。
星号表示指针属于变量，例如：`NSString *text` 不要写成 `NSString* text` 或者 `NSString * text` ，常量除外。
尽量定义属性来代替直接使用实例变量。除了初始化方法（`init`， `initWithCoder:`，等）， `dealloc` 方法和自定义的 setters 和 getters 内部，应避免直接访问实例变量。更多有关在初始化方法和 dealloc 方法中使用访问器方法的信息，参见[这里][Variables_1]。


**推荐：**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**反对：**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

[Variables_1]:https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6

#### 变量限定符

当涉及到[在 ARC 中被引入][Variable_Qualifiers_1]变量限定符时，
限定符 (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) 应该位于星号和变量名之间，如：`NSString * __weak text`。

[Variable_Qualifiers_1]:(https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4)

## 命名

尽可能遵守苹果的命名约定，尤其那些涉及到[内存管理规则][Naming_1]，（[NARC][Naming_2]）的。

长的和描述性的方法名和变量名都不错。

**推荐：**

```objc
UIButton *settingsButton;
```

**反对：**

```objc
UIButton *setBut;
```
类名和常量应该始终使用三个字母的前缀（例如 `NYT`），但 Core Data 实体名称可以省略。为了代码清晰，常量应该使用相关类的名字作为前缀并使用驼峰命名法。

**推荐：**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**反对：**

```objc
static const NSTimeInterval fadetime = 1.7;
```

属性和局部变量应该使用驼峰命名法并且首字母小写。

为了保持一致，实例变量应该使用驼峰命名法命名，并且首字母小写，以下划线为前缀。这与 LLVM 自动合成的实例变量相一致。
**如果 LLVM 可以自动合成变量，那就让它自动合成。**

**推荐：**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**反对：**

```objc
id varnm;
```

[Naming_1]:https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html

[Naming_2]:http://stackoverflow.com/a/2865194/340508

## 注释

当需要的时候，注释应该被用来解释 **为什么** 特定代码做了某些事情。所使用的任何注释必须保持最新否则就删除掉。

通常应该避免一大块注释，代码就应该尽量作为自身的文档，只需要隔几行写几句说明。这并不适用于那些用来生成文档的注释。


## init 和 dealloc

`dealloc` 方法应该放在实现文件的最上面，并且刚好在 `@synthesize` 和 `@dynamic` 语句的后面。在任何类中，`init` 都应该直接放在 `dealloc` 方法的下面。

`init` 方法的结构应该像这样：

```objc
- (instancetype)init {
    self = [super init]; // 或者调用指定的初始化方法
    if (self) {
        // Custom initialization
    }

    return self;
}
```

## 字面量

每当创建 `NSString`， `NSDictionary`， `NSArray`，和 `NSNumber` 类的不可变实例时，都应该使用字面量。要注意 `nil` 值不能传给 `NSArray` 和 `NSDictionary` 字面量，这样做会导致崩溃。

**推荐：**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**反对：**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## CGRect 函数

当访问一个 `CGRect` 的 `x`， `y`， `width`， `height` 时，应该使用[`CGGeometry` 函数][CGRect-Functions_1]代替直接访问结构体成员。苹果的 `CGGeometry` 参考中说到：

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**推荐：**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**反对：**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

[CGRect-Functions_1]:http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html

## 常量

常量首选内联字符串字面量或数字，因为常量可以轻易重用并且可以快速改变而不需要查找和替换。常量应该声明为 `static` 常量而不是 `#define` ，除非非常明确地要当做宏来使用。

**推荐：**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**反对：**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## 枚举类型

当使用 `enum` 时，建议使用新的基础类型规范，因为它具有更强的类型检查和代码补全功能。现在 SDK 包含了一个宏来鼓励使用使用新的基础类型 - `NS_ENUM()`

**推荐：**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## 位掩码

当用到位掩码时，使用 `NS_OPTIONS` 宏。

**举例：**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
    NYTAdCategoryAutos      = 1 << 0,
    NYTAdCategoryJobs       = 1 << 1,
    NYTAdCategoryRealState  = 1 << 2,
    NYTAdCategoryTechnology = 1 << 3
};
```


## 私有属性

私有属性应该声明在类实现文件的延展（匿名的类目）中。

**推荐：**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## 图片命名

图片名称应该被统一命名以保持组织的完整。它们应该被命名为一个说明它们用途的驼峰式字符串，其次是自定义类或属性的无前缀名字（如果有的话），然后进一步说明颜色 和/或 展示位置，最后是它们的状态。

**推荐：**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` 和 `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` 和 `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

图片目录中被用于类似目的的图片应归入各自的组中。


## 布尔

因为 `nil` 解析为 `NO`，所以没有必要在条件中与它进行比较。永远不要直接和 `YES` 进行比较，因为 `YES` 被定义为 1，而 `BOOL` 可以多达 8 位。

这使得整个文件有更多的一致性和更大的视觉清晰度。

**推荐：**

```objc
if (!someObject) {
}
```

**反对：**

```objc
if (someObject == nil) {
}
```

-----

**对于 `BOOL` 来说, 这有两种用法:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**反对：**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // 永远别这么做
```

-----

如果一个 `BOOL` 属性名称是一个形容词，属性可以省略 “is” 前缀，但为 get 访问器指定一个惯用的名字，例如：

```objc
@property (assign, getter=isEditable) BOOL editable;
```

内容和例子来自 [Cocoa 命名指南][Booleans_1] 。

[Booleans_1]:https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE


## 单例

单例对象应该使用线程安全的模式创建共享的实例。

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
这将会预防[有时可能产生的许多崩溃][Singletons_1]。

[Singletons_1]:http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html

## 导入   

如果有一个以上的 import 语句，就对这些语句进行[分组][Import_1]。每个分组的注释是可选的。   
注：对于模块使用 [@import][Import_2] 语法。   

```objc   
// Frameworks
@import QuartzCore;

// Models
#import "NYTUser.h"

// Views
#import "NYTButton.h"
#import "NYTUserView.h"
```   


[Import_1]: http://ashfurrow.com/blog/structuring-modern-objective-c
[Import_2]: http://clang.llvm.org/docs/Modules.html#using-modules

## Xcode 工程

为了避免文件杂乱，物理文件应该保持和 Xcode 项目文件同步。Xcode 创建的任何组（group）都必须在文件系统有相应的映射。为了更清晰，代码不仅应该按照类型进行分组，也可以根据功能进行分组。


如果可以的话，尽可能一直打开 target Build Settings 中 "Treat Warnings as Errors" 以及一些[额外的警告][Xcode-project_1]。如果你需要忽略指定的警告,使用 [Clang 的编译特性][Xcode-project_2] 。


[Xcode-project_1]:http://boredzo.org/blog/archives/2009-11-07/warnings

[Xcode-project_2]:http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas


# 其他 Objective-C 风格指南

如果感觉我们的不太符合你的口味，可以看看下面的风格指南：

* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)

