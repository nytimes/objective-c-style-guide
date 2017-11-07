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
* [位元碼](#位元碼)
* [私有屬性](#私有屬性)
* [圖片命名](#圖片命名)
* [布林值](#布林值)
* [單例](#單例)
* [引入](#引入)
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

**推薦：**
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


## 條件判斷

條件判斷的主體應該始終使用大括號來防止[出錯][Condiationals_1]，即使它可以不用大括號（例如它只需要一行）。會發生的錯誤包含添加第二行程式碼並希望它是if語句的一部分。還有一種[更危險的][Condiationals_2]，當if語句裡面的一行被註解掉，下一行就會在不經意間成為了這個if語句的一部分。此外，這種風格也更符合所有其他的條件判斷，因此也更容易檢查。

**推薦：**
```objc
if (!error) {
    return success;
}
```

**不推薦：**
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

### 三元運算子

三元運算子，? ，只有當它可以增加程式碼清晰度或整潔時才使用。單一的條件時應該優先考慮使用，多條件時通常使用if語句會更容易理解，或是定義為變數。

**推薦：**
```objc
result = a > b ? x : y;
```

**不推薦：**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## 錯誤處理

當引用一個返回錯誤參數（error parameter）的方法時，應該針對返回值，而非錯誤變數。

**推薦：**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // 处理错误
}
```

**不推薦：**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // 处理错误
}
```
一些Apple的API在成功的情況下會寫一些垃圾數值給錯誤參數（如果不是空值），所以針對錯誤變數時可能會造成錯誤結果（以及接下来的Crash）。

## 方法

在方法命名中，在 -/+ 符號後應該有一個空格。方法片段之間也應該有一個空格。

**推薦：**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## 變數

變數名稱應該儘可能描述性的。除了 `for()` 迴圈外，其他情況都應該避免使用單字母的變數名。
星號表示指針屬於變數，例如：`NSString *text` 不要寫成 `NSString* text` 或者 `NSString * text` ，常數除外。
儘量定義屬性來代替直接使用實體變數。除了初始化方法（`init`， `initWithCoder:`，等）， `dealloc` 方法和自定義的 setters 和 getters ，應該避免直接訪問實體變量。更多有關在初始化方法和dealloc方法中使用訪問氣的方法，參見[這裏][Variables_1]。

**推薦：**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**不推薦：**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

[Variables_1]:https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6

#### 變數限定符號

當涉及到[在 ARC 中被引入][Variable_Qualifiers_1]變數限定符號時，
限定符號 (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) 應該位於星號和變數名之間，如：`NSString * __weak text`。

[Variable_Qualifiers_1]:(https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4)

## 命名

盡可能遵守Apple的命名規定，尤其那些涉及到[內存管理規則][Naming_1]，（[NARC][Naming_2]）的。

長的和描述性的方法名和變數名都不錯。

**推薦：**

```objc
UIButton *settingsButton;
```

**不推薦：**

```objc
UIButton *setBut;
```
類別名和常數應該使用三個字母的前綴（例如 `NYT`），但Core Data實體名稱可以省略，為了代碼清晰，常數應該使用相關類別的名字做為前綴並使用駝峰命名法。

**推薦：**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**不推薦：**

```objc
static const NSTimeInterval fadetime = 1.7;
```

屬性和局部變量應該使用駝峰命名法並且首字母小寫。

為了保持一致，實體變數應該使用駝峰命名法命名，並且首字母小寫，以下畫底線為前綴。這與LLVM自動合成的實體變數一致。
**如果 LLVM 可以自動合成變數，那就讓它自動合成。**

**推薦：**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**不推薦：**

```objc
id varnm;
```

[Naming_1]:https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html

[Naming_2]:http://stackoverflow.com/a/2865194/340508

## 註解

當需要的時候，註解應該被用來解釋 **為什麼** 特定程式碼做了某些事情。所使用的任何註解必須保持最新否則就刪除掉。

通常應該避免一大塊註解，程式碼應該盡量作為描述自身的文檔，只需要隔幾行寫幾句說明。這並不適用那些用來生成文檔的註解。


## init 和 dealloc

`dealloc` 方法應該放在實現文件的最上面，並且剛好在 `@synthesize` 和 `@dynamic` 語句的後面。在任何類別中，`init` 都應該直接放在 `dealloc` 方法的下面。

`init` 方法的結構應該像這樣：

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

每當建立 `NSString`， `NSDictionary`， `NSArray`，和 `NSNumber` 的不可變實體時，，都應該使用字面量。要注意 `nil` 值不能傳給 `NSArray` 和 `NSDictionary` 字面量，這樣做會導致Crash。

**推薦：**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**不推薦：**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## CGRect 函數

當訪問一個 `CGRect` 的 `x`， `y`， `width`， `height` 時，應該使用[`CGGeometry` 函數][CGRect-Functions_1]代替直接訪問結構體成員。Apple的 `CGGeometry` 參考說道：

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**推薦：**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**不推薦：**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

[CGRect-Functions_1]:http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html

## 常數

常數首選內聯的字串字面量或數字，因為常數可以輕易重用並且可以快速改變而不需要查找和替換。常數應該聲明為 `static` 常數而不是 `#define` ，除非非常明確地要當作巨集來使用。

**推薦：**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**不推薦：**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## 列舉型別

當使用 `enum` 時，建議使用新的基礎列舉型別規範，因為它具有更強的型別檢查和程式碼補全功能。現在SDK包含了一個巨集來鼓勵使用新的基礎型別 - `NS_ENUM()`

**推薦：**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## 位元碼

當用到位元碼時，使用 `NS_OPTIONS` 巨集。

**舉例：**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
    NYTAdCategoryAutos      = 1 << 0,
    NYTAdCategoryJobs       = 1 << 1,
    NYTAdCategoryRealState  = 1 << 2,
    NYTAdCategoryTechnology = 1 << 3
};
```


## 私有屬性

私有屬性應該聲明在類別的延伸中（匿名的類目）。

**推薦：**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## 圖片命名

圖片名稱應該被統一命名以保持組織的完整性。它們應該被命名為一個說明它們用途的駝峰式字串，其次是自定義類別或屬性的無前綴名字（如果有的話），然後進一步說明顏色和/或展示位置，最後是它們的狀態。

**推薦：**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` 和 `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` 和 `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

圖片目錄中被用於類似目的的圖片應被放入各自類別的資料夾中。


## 布林值

因為 `nil` 解析為 `NO`，所以沒有必要在條件中與它進行比較。永遠不要直接和 `YES` 進行比較，因為 `YES` 被定義為1，而在Objective-C裡 `BOOL` 是一個 `CHAR` 的類別並达 8 位元。（所以數值 `11111110` 跟 `YES` 比較時，會回傳 `NO`）

**推薦：**

```objc
if (!someObject) {
}
```

**不推薦：**

```objc
if (someObject == nil) {
}
```

-----

**對於 `BOOL` 來說, 有兩種用法:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**不推薦：**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // 永遠別這樣做
```

-----

如果一個 `BOOL` 屬性名稱是一個形容詞，屬性可以省略 `is` 前綴，但為 get 訪問器指定一個慣用名字，例如：

```objc
@property (assign, getter=isEditable) BOOL editable;
```

内容和例子来自 [Cocoa 命名指南][Booleans_1] 。

[Booleans_1]:https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE


## 單例

單例對象應該使用線程安全的模式建立共同的實體。

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
這將會預防[有時可能產生的許多Crash][Singletons_1]。

[Singletons_1]:http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html

## 引入

如果有一個以上的import語法，就對這些語法進行[分組][Import_1]。可在每組上加上註解。
注：對於模組使用 [@import][Import_2] 語法。

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

為了避免文件雜亂，物理文件應該保持和Xcode項目文件同步。Xcode 建立的任何群組（group）都必須在文件系統上有著相對應的資料夾。為了更清晰，程式碼不僅應該按照類型進行分組，也可以根據功能進行分組。


如果可以的話，盡可能一直打開 target Build Settings 中 "Treat Warnings as Errors" 以及一些[額外的警告][Xcode-project_1]。如果你需要忽略指定的警告,使用 [Clang 的編譯特性][Xcode-project_2] 。


[Xcode-project_1]:http://boredzo.org/blog/archives/2009-11-07/warnings

[Xcode-project_2]:http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas


# 其他 Objective-C 風格指南

如果感覺我們不太符合你的口味，可以看看下面的風格指南：

* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)

