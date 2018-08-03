# NYTimes Objective-C スタイルガイド

このスタイルガイドは、ニューヨーク・タイムズでの iOS チームのコーディング規約です。[issues](https://github.com/NYTimes/objetive-c-style-guide/issues)、[pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls) 、[つぶやき](https://twitter.com/nytimesmobile)へのフィードバックを歓迎します。また、[人材募集中](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY-10001/73366300/)です。

[貢献者の皆さん](https://github.com/NYTimes/objective-c-style-guide/contributors)に感謝します。

## はじめに

Appleからいくつかのスタイルガイド情報が提供されています。ここで述べられていないことは、それらを参考にしてください:

* [The Objective-C Programming Language](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/legacy/library/documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## 目次

* [ドット表記構文](#ドット表記構文)
* [スペーシング](#スペーシング)
* [条件文](#条件文)
  * [三項演算子](#三項演算子)
* [エラー処理](#エラー処理)
* [メソッド](#メソッド)
* [変数](#変数)
* [命名](#命名)
  * [カテゴリ](#カテゴリ)
* [コメント](#コメント)
* [初期化と解放](#初期化と解放)
* [リテラル](#リテラル)
* [CGRect 関数](#cgrect-関数)
* [定数](#定数)
* [列挙型](#列挙型)
* [ビットマスク](#ビットマスク)
* [プライベートプロパティ](#プライベートプロパティ)
* [画像ファイル名](#画像ファイル名)
* [ブーリアン](#ブーリアン)
* [シングルトン](#シングルトン)
* [インポート](#インポート)
* [Xcode プロジェクト](#xcode-プロジェクト)

## ドット表記構文

プロパティのアクセスや変更する際に**必ず**ドット表記を使うこと。それ以外の場合は大括弧表記を使うこと。

**良い例：**
```objc
view.backgroundColor = UIColor.orangeColor;
UIApplication.sharedApplication.delegate;
```

**悪い例：**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
[UIApplication sharedApplication].delegate;
```

## スペーシング

* スペース4つでインデントすること。タブキーでインデントしない。Xcodeで設定するように。
* メソッドの中括弧とその他の中括弧(`if`/`else`/`switch`/`while`など)は始め中括弧を同じ行で書き、終わり中括弧を改行で書くこと。

**良い例：**
```objc
if (user.isHappy) {
    // 何かをする
}
else {
    // 他の何かをする
}
```
* 見た目を分かりやすくするために、メソッド間は一行開けること。
* メソッド内の機能を分けるために一行開けること(ただ、これはメソッドをより小さく分割すべきという兆候でもある)。長く詳細な名前のメソッドでは、見た目を分けるためにメソッド名と中身の間を一行開けること。
* `@synthesize`と`@dynamic`は一行ずつ宣言すること。

## 条件文

条件文は常に中括弧を使うこと。たとえそれが中括弧なしで書ける場合(例えば、一行文)であってもである。それによって、[間違い](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256)を防ぐことができる。ありがちな間違いとして、行を追記する際に直前のif文に追加されたものとする勘違いがある。他にも、if文内の1行がコメントアウトされると、次の1行がうっかりif文の一部となってしまうという[より危険な問題](http://programmers.stackexchange.com/a/16530)もある。このスタイルの良い点として、他の全ての条件文と見た目が一貫するため、読み取りやすくなる。

**良い例：**
```objc
if (!error) {
    return success;
}
```

**悪い例：**
```objc
if (!error)
    return success;
```

または

```objc
if (!error) return success;
```

### 三項演算子

三項演算子 `?` は分かりやすくなる場合のみ使うこと。

**良い例：**
```objc
result = a > b ? x : y;
```

**悪い例：**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## エラー処理

メソッドがエラーの返戻値の参照を返す場合、条件はエラー変数を使わずに、返戻値を使うこと。

**良い例：**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // エラー処理
}
```

**悪い例：**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // エラー処理
}
```

AppleのAPIのいくつかは、成功時にエラー変数へゴミ値を入れる。そのためエラー変数を条件に使う場合、エラー発生したと誤判定される(そしてクラッシュする)。

## メソッド

メソッドシグネチャでは、メソッドのスコープ(`-`や`+`シンボル)の後にスペースを開けること。メソッドのセグメント間にスペースを開けること。

**良い例：**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## 変数

変数名をわかりやすく命名すること。その変数が表すものやプログラマーに必要な関連情報を提供することにより、値を適切に使えるようになります。

**良い例：**

* `NSString *title`: “title”を文字列とするのは妥当です。
* `NSString *titleHTML`: 表示用にパースする必要のあるHTMLを含んだタイトルのこと。_プログラマーがこの変数を効果的に利用するために“HTML”が付加されています。_
* `NSAttributedString *titleAttributedString`: 表示用に整形済みのタイトル。 _`AttributedString`から、この値がただのありきたりなタイトルではないことがわかります。これを加えることは文脈に則していて妥当であるといえます。_
* `NSDate *now`: _これ以上の分かりやすさはないでしょう。_
* `NSDate *lastModifiedDate`: 単純に`lastModified` だけだと曖昧です。しかし文脈に応じて明確になります。
* `NSURL *URL` 対 `NSString *URLString`: ある値が異なるクラスにより表現される場合、それを変数名にすることで分かりやすくなります。
* `NSString *releaseDateString`: ある値が別のクラスにより表現される別例です。この名前により分かりやすくなっています。

ループ中の単純なカウンター変数以外では、一文字の変数名は使わないこと。

ポインタ型で使うアスタリスクは、変数名側に付けること。**良い例**は、`NSString *text`です。**悪い例**は、`NSString* text`や`NSString * text`です。ただし定数は除く(`NSString * const NYTConstantString`)。

できれば素のインスタンス変数を使わずにプロパティで宣言すること。インスタンス変数への直接アクセスは避けること。ただし初期化メソッド(`init`、`initWithCoder:`、その他…)、`dealloc`メソッド、setterとgetterを除く。詳細は[初期化メソッドと`dealloc`内のアクセサメソッド利用に関するAppleの文書](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)を参照すること。

**良い例：**

```objc
@interface NYTSection : NSObject

@property (nonatomic) NSString *headline;

@end
```

**悪い例：**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

#### 変数修飾子

[ARCにより導入された](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4)変数修飾子は、(`__strong`、`__weak`、`__unsafe_unretained`、`__autoreleasing`)アスタリスクと変数名の間におくこと。例えば`NSString * __weak text`。

## 命名

できればAppleの命名規約に従うこと。特にこれは[メモリ管理ルール](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508))に関連があります。

メソッド名や変数名は、長くてわかりやすいのが良い。

**良い例：**

```objc
UIButton *settingsButton;
```

**悪い例：**

```objc
UIButton *setBut;
```

三文字接頭辞(例えば`NYT`)をクラス名や定数名にいつも使うこと。ただし、Core Dataエンティティ名は除く。定数名はキャメルケースで全ての先頭文字を大文字にし、関連クラス名を接頭辞にすること。二文字接頭辞(例えば`NS`)は[Appleに予約されています](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/DefiningClasses/DefiningClasses.html#//apple_ref/doc/uid/TP40011210-CH3-SW12)。

**良い例：**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**悪い例：**

```objc
static const NSTimeInterval fadetime = 1.7;
```

プロパティ名とローカル変数名は、キャメルケースで最初の文字を小文字にすること。

インスタンス変数はキャメルケースで最初の文字を小文字にし、アンダースコアを接頭辞にすること。これにより、LLVMが自動的にsynthesizeしたインスタンス変数と一貫性を保つことができます。**LLVMの自動`synthesize`はそのままにしておくこと。**

**良い例：**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**悪い例：**

```objc
id varnm;
```

### カテゴリ

カテゴリは機能を簡潔に表現する名前にすること。

**良い例：**

```objc
@interface UIViewController (NYTMediaPlaying)
@interface NSString (NSStringEncodingDetection)
```

**悪い例：**

```objc
@interface NYTAdvertisement (private)
@interface NSString (NYTAdditions)
```

## コメント

コメントは必要の場合のみ書く。処理の**理由**を説明すること。コメントの更新することを忘れずに。必要がなくなったら削除すること。

ブロックコメントをなるべく書かないこと。できるだけコード自身に自己説明させること。ドキュメント生成用のコメントは例外。

## 初期化と解放

`dealloc`メソッドは実装部の最初、`@synthesize`や`@dynamic`宣言の直後に書くこと。`init`は`dealloc`メソッドの直後に書くこと。

`init`メソッドを次のように書くこと。

```objc
- (instancetype)init {
    self = [super init]; // または指定の初期化メソッド
    if (self) {
        // カスタマイズ初期化処理
    }
    return self;
}
```

## リテラル

`NSString`、`NSDictionary`、`NSArray`、`NSNumber`リテラルを使うこと。これらのオブジェクトの不変インスタンスを作る場合は常にです。`nil`を`NSArray`や`NSDictionary` リテラルに入れないこと。クラッシュします。

**良い例：**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**悪い例：**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## `CGRect` 関数

`CGRect`の`x`,`y`,`width`,`height`にアクセスする際、構造体メンバーに直接アクセスせずに**必ず** [`CGGeometry`関数](https://developer.apple.com/documentation/coregraphics/cggeometry)を使うこと。Appleの`CGGeometry` レファレンスより：

> このレファレンス中の関数でCGRectデータ構造を入力とするものは、矩形を暗黙的に標準化してから結果を計算する。そのため、アプリケーションはCGRectデータ構造内のデータを直接読み書きすべきではない。かわりに、ここに記載されている関数を使って矩形計算し、値を取得すること。

**良い例：**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**悪い例：**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## 定数

インライン文字列や数値をなるべく定数で宣言するように。その方があとで変更しやすい。定数を`#define`ではなく`static`で宣言するように。ただし明らかにマクロで使われる場合は除く。

**良い例：**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**悪い例：**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## 列挙型

列挙型定数を宣言する際、`enum`ではなく新しい列挙型`NS_ENUM()`を使うように。

**良い例：**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## ビットマスク

ビットマスクを使う際、`NS_OPTIONS`マクロを使うこと。

**良い例：**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
  NYTAdCategoryAutos      = 1 << 0,
  NYTAdCategoryJobs       = 1 << 1,
  NYTAdCategoryRealState  = 1 << 2,
  NYTAdCategoryTechnology = 1 << 3
};
```

## プライベートプロパティ

プライベートプロパティは実装ファイルのクラス拡張に書き、カテゴリ名を空にすること。

**良い例：**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## 画像ファイル名

画像ファイル名は一貫した名前をつけること。キャメルケースで目的名、続いて接頭語無しのクラス名やプロパティ名、続いて色名や位置名、最後に状態名という名前にする。

**良い例：**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` と `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` と `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

ImagesフォルダやAsset Catalogに同じ目的の画像をグループで分ける。

## ブーリアン

`YES`と直接比較しないこと。なぜなら`YES`は`1`と定義されていて、Objective-Cの`BOOL`は8ビット長の`CHAR`型のため(そのため`11111110`は`YES`と比較したら`NO`が返る)。

**オブジェクトポインタの良い例：**

```objc
if (!someObject) {
}

if (someObject == nil) {
}
```

**`BOOL`値の良い例：**

```objc
if (isAwesome)
if (!someNumber.boolValue)
if (someNumber.boolValue == NO)
```

**悪い例：**

```objc
if (isAwesome == YES) // 決してしないこと。
```

`BOOL`プロパティ名が形容詞なら、`is`プレフィックスが省略可だがゲッター名にて指定するように。

**良い例：**

```objc
@property (assign, getter=isEditable) BOOL editable;
```

_テキストと例は[Cocoa命名ガイドライン](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)より引用。_

## シングルトン

スレッドセーフパターンでシングルトンを作成するように。
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
これにより、[クラッシュの可能性](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html)を避けることができます。

## インポート

インポート文が複数ある場合、[グループにすること](http://ashfurrow.com/blog/structuring-modern-objective-c)。それぞれのグループにコメントしてもよい。

注意：モジュールには[@import](http://clang.llvm.org/docs/Modules.html#using-modules)構文を使うこと。

```objc
// Frameworks
@import QuartzCore;

// Models
#import "NYTUser.h"

// Views
#import "NYTButton.h"
#import "NYTUserView.h"
```

## Xcode プロジェクト

Xcodeのグループとプロジェクトのフォルダが同じようにして分かりやすくすること。型でグループするだけではなく、機能ごとにグループするように。

できれば、targetのBuild Settingsの“Treat Warnings as Errors“をオンにし、多めに[アディショナルワーニング](http://boredzo.org/blog/archives/2009-11-07/warnings) をオンにする。特定のワーニングをオフしたい場合、[Clang's pragma機能](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas)を使うように。

# 他のObjective-Cスタイルガイド

もし我々のスタイルガイドが合わない場合、他のスタイルガイドをチェックしてみてください。

* [Google](https://google.github.io/styleguide/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
* [Wikimedia](https://www.mediawiki.org/wiki/Wikimedia_Apps/Team/iOS/ObjectiveCStyleGuide)
