# NYTimes Objective-C Style Guide [원문](https://github.com/NYTimes/objective-c-style-guide)

이 스타일 가이드는 New York Times의 iOS팀 코딩 규칙을 설명합니다. [issues](https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls)와 [tweets](https://twitter.com/nytimesmobile)에 당신의 의견을 환영합니다. 또한, [구인 중입니다](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY/2572221/).

[공헌자](https://github.com/NYTimes/objective-c-style-guide/contributors) 모두에게 감사드립니다.


## <a name='Introduction'>소개</a>

세련된 애플 스타일 가이드 문서 일부가 여기에 있습니다. 여기에 언급된 것이 없다면, 다음 중 하나에서 매우 자세히 다뤘을 겁니다:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)


## <a name='TOC'>목차</a>

* [Dot-Notation Syntax](#dot-notation-syntax)
* [Spacing](#spacing)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Error handling](#error-handling)
* [Methods](#methods)
* [Variables](#variables)
* [Naming](#naming)
* [Comments](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Literals](#literals)
* [CGRect Functions](#cgrect-functions)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Bitmasks](#bitmasks)
* [Private Properties](#private-properties)
* [Image Naming](#image-naming)
* [Booleans](#booleans)
* [Singletons](#singletons)
* [Xcode Project](#xcode-project)


## <a name='dot-notation-syntax'>Dot-Notation Syntax</a> [원문](https://github.com/NYTimes/objective-c-style-guide#dot-notation-syntax)

점 표기법은 **항상** 프로퍼티를 접근하거나 변경할 때 사용합니다. 대괄호 표기법은 그밖에 다른 모든 경우에 바람직합니다.

**For example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## <a name='spacing'>Spacing</a> [원문](https://github.com/NYTimes/objective-c-style-guide#spacing)

* 들여쓰기는 공백 4개를 사용합니다. 절대로 탭으로 들여쓰기를 하지 않습니다. Xcode에서 preference를 설정해야 합니다.
* 메소드 중괄호와 다른 중괄호 (`if`/`else`/`switch`/`while` etc.)는 항상 같은 줄에서 문을 열고 새로운 줄에서 닫습니다.

**For example:**
```objc
if (user.isHappy) {
//Do something
}
else {
//Do something else
}
```

* 시각적 선명도와 조직을 도와주기 위해 메소드 사이에 빈 줄이 있어야 합니다. 메소드 내에서 공백은 기능을 분리할 수 있지만, 종종 새로운 메소드가 될 수도 있습니다.
* `@synthesize`와 `@dynamic`는 implementation에 새로운 줄로 각각 선언해야 합니다.


## <a name='conditionals'>Conditionals</a> [원문](https://github.com/NYTimes/objective-c-style-guide#conditionals)

조건문 본체는 중괄호가 없이 작성할 때(한 줄인 경우) [에러](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256)를 방지하기 위해 항상 중괄호를 사용해야 합니다. 이러한 오류는 추가된 두 번째 줄이 if-문의 한 부분으로 예상하여 발생합니다. 또한, 줄 내부 if-문을 주석처리하고 다음 줄이 if-문의 한 부분으로 무심코 되는 곳에서 [더 심각한 문제](http://programmers.stackexchange.com/a/16530)가 발생할 수 있습니다. 게다가 이러한 스타일은 그 밖에 모든 조건문과 더욱 일치하기 때문에 더욱 쉽게 찾을 수 있습니다.

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


### <a name='ternary-operator'>Ternary Operator</a> [원문](https://github.com/NYTimes/objective-c-style-guide#conditionals)

3항 연산자 ?는 명료함과 코드 간결함을 높일 때 사용해야 합니다. 보통 단일 조건은 모든 것을 비교해야 합니다. 일반적으로 다중 조건 비교는 if 문 또는 인스턴스 변수로 리펙토링을 통해 더 쉽게 이해할 수 있습니다.

**For example:**
```objc
result = a > b ? x : y;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```


## <a name='error-handling'>Error handling</a> [원문](https://github.com/NYTimes/objective-c-style-guide#error-handling)

메소드가 참조로 에러 파라미터를 반환을 때, 에러 변수로 전환하는 것이 아니라 반환된 값으로 변경합니다.

**For example:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}
```

**Not:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Apple API 일부는 성공인 경우 에러 파라미터(NULL이 아니라면)에 쓰래기 값을 기록합니다. 그래서 에러로 바꾸는 것은 오류 (그리고 이후 충돌)를 야기할 수 있습니다.


## <a name='methods'>Methods</a> [원문](https://github.com/NYTimes/objective-c-style-guide#methods)

메소드 서명에 범위(-/+ 기호)뒤에 공백이 있어야 합니다. 메소드 마디 사이에 공백이 있어야 합니다.

**For Example**:
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## <a name='variables'>Variables</a> [원문](https://github.com/NYTimes/objective-c-style-guide#variables)

변수는 서술이 가능하도록 명명되어야 합니다. 단일 문자 변수 이름은 `for()` 문을 제외하곤 피해야 합니다.

예를 들어 상수의 경우를 제외하고 포인터를 나타내는 별표와 함께 있는 변수는 `NSString *text` 이며  `NSString* text` 또는 `NSString * text`는 아닙니다.

프로퍼티 정의는 노출된 인스턴스 변수에 위치하여 언제든지 사용 가능합니다. 직접 인스턴스 변수는 초기화 메소드(`init`, `initWithCoder:`, etc…), `dealloc` 메소드와 내부 커스텀 setter와 getter를 제외하고 피해야 합니다. 초기화 메소드와 dealloc에 접근자 메소드를 사용하려면 [여기](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)에서 더 많은 정보를 볼 수 있습니다.

**For example:**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Not:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

## <a name='naming'>Naming</a> [원문](https://github.com/NYTimes/objective-c-style-guide#naming)

애플 명명 규칙은 가능하면 준수해야 하며, 특히 [메모리 관리 규칙](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)([NARC](http://stackoverflow.com/a/2865194/340508))과 관련있습니다.

길고 상세한 메소드와 변수명이 좋습니다.

**For example:**

```objc
UIButton *settingsButton;
```

**Not**

```objc
UIButton *setBut;
```

세 글자 접두사(예, `NYT`)는 클래스 이름과 상수에 사용하지만, Core Data entity 이름에는 생략될 수 있습니다. 상수는 대문자와 명확성을 위한 연관된 클래스 이름을 접두사로 하는 모든 단어를 낙타 표기법으로 해야 합니다.

**For example:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Not:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

프로퍼티와 로컬 변수는 앞에 단어가 소문자로 하는 낙타 표기법으로 해야 합니다.

인스턴스 변수는 첫 단어는 소문자로 낙타 표기법이며 밑줄로 시작합니다. LLVM에 의해 자동으로 합성된 인스턴스 변수와 일치합니다. **만약 LLVM이 자동으로 변수를 합성할 수 있다면 다음을 봅시다.**

**For example:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**

```objc
id varnm;
```

## <a name='comments'>Comments</a> [원문](https://github.com/NYTimes/objective-c-style-guide#comments)

필요한 경우 주석은 코드의 특정 부분이 어떤 행동하는지 **이유**를 설명해야 합니다. 주석은 최신 상태로 유지하거나 삭제해야 합니다.

주석 블럭을 만드는 것은 피해야 하며, 코드는 가능하면 스스로 문서가 되도록 해야 하고, 필요할 때 몇 줄로 설명해야 합니다.

## <a name='init-and-dealloc'>init and dealloc</a> [원문](https://github.com/NYTimes/objective-c-style-guide#init-and-dealloc)

`dealloc` 메소드는 implementation 상단, `@synthesize` 와 `@dynamic`문 뒤에 바로 위치해야 합니다. `init`은 클래스 메소드 `dealloc` 밑에 바로 위치해야 합니다.

`init` 메소드는 다음과 같이 구성되어 있습니다:

```objc
- (instancetype)init {
    self = [super init]; // or call the designated initalizer
    if (self) {
        // Custom initialization
    }

    return self;
}
```

## <a name='literals'>Literals</a> [원문](https://github.com/NYTimes/objective-c-style-guide#literals)

`NSString`, `NSDictionary`, `NSArray`, `NSNumber` 리터럴은 불변 인스턴스 객체를 생성하는데 사용합니다. Pay는 `nil` 값이 `NSArray` 와 `NSDictionary` 리터럴에 통과되지 않도록 특히 신중해야 합니다. 이는 충돌이 발생할 수 있기 때문입니다.

**For example:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Not:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## <a name='cgrect-functions'>CGRect Functions</a> [원문](https://github.com/NYTimes/objective-c-style-guide#cgrect-functions)


`CGRect`의 `x`, `y`, `width` 또는 `height`를 접근하는 경우, 직접 struct 멤버에 접근하지말고 [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html)를 항상 사용하세요. 애플의 `CGGeometry` 참조 : 


> 결과를 계산하기 전에 표준화된 사각형에 내제되어 입력된 CGRect 데이타 구조를 가지는 참조에서 모든 함수가 설명됩니다. 이러한 이유로 어플리케이션은 CGRect 데이타 구조에 저장된 데이타를 직접 읽고 쓰는 것을 피해야 합니다. 대신 사각형을 다루고 그 특성을 검색하도록 여기에 기술된 함수를 사용합니다.

**For example:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## <a name='constants'>Constants</a> [원문](https://github.com/NYTimes/objective-c-style-guide#constants)

상수는 일반적으로 사용되는 변수는를 쉽게 재생산이 가능하며 검색하고 교체할 필요 없이 빠르게 변경하므로 인라인 문자열 리터럴 또는 숫자보다 선호합니다. 상수는 `static` 상수로 선언해야 하며 명시적으로 매크로로 사용하지 않으면 `#define` 상수로 선언하지 않습니다.

**For example:**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**Not:**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## <a name='enumerated-types'>Enumerated Types</a> [원문](https://github.com/NYTimes/objective-c-style-guide#enumerated-types)

`enum`을 사용하는 경우, 새롭게 수정된 기본 형식 명세서를 사용하는 것을 추천합니다. 타입 확인과 코드 완성을 더욱 강력하게 해줍니다. SDK는 수정된 기본 형식 - `NS_ENUM()`을 수월하게 사용할 수 있는 매크로를 포함하고 있습니다.

**Example:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## <a name='bitmasks'>Bitmasks</a> [원문](https://github.com/NYTimes/objective-c-style-guide#bitmasks)

bitmask로 작업하는 경우 `NS_OPTIONS` 매크로를 사용합니다.

**Example:**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
  NYTAdCategoryAutos      = 1 << 0,
  NYTAdCategoryJobs       = 1 << 1,
  NYTAdCategoryRealState  = 1 << 2,
  NYTAdCategoryTechnology = 1 << 3
};
```

## <a name='private-properties'>Private Properties</a> [원문](https://github.com/NYTimes/objective-c-style-guide#private-properties)

비공개 프로퍼티는 클래스의 implementation 파일안에 클래스 익스텐션(익명 카테고리)에 선언합니다. 명시된 카테고리(`NYTPrivate` 또는 `private` 같은)는 다른 클래스를 확장 하지 않는 한 절대로 사용하지 마세요.

**For example:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## <a name='image-naming'>Image Naming</a> [원문](https://github.com/NYTimes/objective-c-style-guide#image-naming)

이미지 이름은 조직과 개발자 정신을 유지하기 위해 일관되게 이름을 정해야 합니다. 이미지의 목적을 설명하기 위해 처음 문자열은 낙타 표기법으로, 다음으로 클래스의 이름이나 사용자 지정 속성, 마지막으로 색상과/또는 위치, 상태의 추가 설명으로 이름을 정해집니다.

**For example:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

이미지는 이미지 폴더에 비슷한 목적을 가진 그룹으로 각각 나누어 사용합니다.

## <a name='booleans'>Booleans</a> [원문](https://github.com/NYTimes/objective-c-style-guide#booleans)

This allows for more consistency across files and greater visual clarity.

`nil`은 `NO`로 해석하기 때문에 조건문에서 비교할 필요가 없습니다. 무언가를 직접적으로 `YES`로 비교하지 마세요. `YES`는 1로 정의되며 `BOOL`은 최대 8비트가 될 수 있습니다.

파일에서 더 많은 일관성과 더 큰 시각적 명확성이 있도록 합니다.

**For example:**

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

**For a `BOOL`, here are two examples:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**Not:**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Never do this.
```

-----

예를 들어 `BOOL` 프로퍼티 이름이 형용사로 나타내면, 프로퍼티는 “is” 접두사는 생략할 수 있지만 get 접근자는 기존 이름을 지정합니다:

```objc
@property (assign, getter=isEditable) BOOL editable;
```

[Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)에서 가져온 글과 예제입니다.

## <a name='singletons'>Singletons</a> [원문](https://github.com/NYTimes/objective-c-style-guide#singletons)

싱글톤 객체는 공유 인스턴스를 생성하기 위해 쓰레드 세이프 패턴을 사용합니다.

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

이것은 [가끔 원인이 되는 충돌](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html)을 방지할 수 있습니다.

## <a name='xcode-project'>Xcode project</a> [원문](https://github.com/NYTimes/objective-c-style-guide#xcode-project)

실제 파일은 무분별한 확산을 방지하기 위해 Xcode 프로젝트 파일과 동기화를 유지해야 합니다. 어떤 Xcode 그룹은 파일 시스템에 폴더를 반영되어 만들어집니다. 코드는 더 큰 명확성을 위해 타입으로만 묶는 것이 아니라 기능과 같이 묶어야 합니다.

가능하면 항상 타켓 빌드 설정에서 "Treat Warnings as Errors"를 켜고 가능한 많은 [추가 경고](http://boredzo.org/blog/archives/2009-11-07/warnings) 활성화 합니다. 특정 경고를 무시해야 한다면 [Clang's pragma 기능](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas) 사용하세요.

# Other Objective-C Style Guides

이 스타일 가이드가 당신 입맛에 맞지 않다면, 다른 스타일 가이드를 보세요.

* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
