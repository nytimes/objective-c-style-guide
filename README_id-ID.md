# NYTimes Objective-C Style Guide

Panduan gaya (style) ini membahas mengenai konvensi pengkodean dari tim iOS di The New York Times. Kami menerima masukan pada [issues](https://github.com/NYTimes/objective-c-style-guide/issues) dan [pull requests](https://github.com/NYTimes/objective-c-style-guide/pulls). Juga, [we’re hiring](http://www.nytco.com/careers/).

Terima kasih kepada semua  [kontributor kami](https://github.com/NYTimes/objective-c-style-guide/graphs/contributors).

## Introduction

Berikut ini adalah beberapa dokymen dari Apple mengenai panduan gaya ini. Jika ada hal yang tidak disebutkan di sini, maka kemungkinan dibahas dengan sangat detail pada salah satu dari link berikut:

* [Bahasa Pemrograman Objective-C](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Panduan Dasar Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Panduan Pembuatan Kode Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [Panduan Pemrograman Aplikasi iOS ](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

Panduan gaya ini mengikuti IETF's [RFC 2119](http://tools.ietf.org/html/rfc2119).Secara umum, kode yang berlawananan dengan gaya RECOMMENDED/SHOULD diperbolehkan, namun harus dipertimbangkan dengan baik.

## Table of Contents

* [Syntax Notasi Dot](#dot-notation-syntax)
* [Spacing](#spacing)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Error handling](#error-handling)
* [Metode](#metode)
* [Variabel](#variabel)
* [Pemberian Nama](#naming)
  * [Kategori](#categories)
* [Komentar](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Literals](#literals)
* [Fungsi CGRect](#cgrect-functions)
* [Konstanta](#constants)
* [Tipe Enumerated](#enumerated-types)
* [Bitmasks](#bitmasks)
* [Private Properties](#private-properties)
* [Pemberian Nama Gambar](#image-naming)
* [Booleans](#booleans)
* [Singletons](#singletons)
* [Imports](#imports)
* [Protokol](#protocols)
* [Proyek Xcode](#xcode-project)

## Dot Notation Syntax

Notasi dot lebih DIREKOMENDASIKAN daripada notasi tanda kurung untuk menngambil dan mengatur properti.

**Contohnya:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Bukan:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Spacing

* Indentasi HARUS 4 spasi. Jangan pernah membirikan indent menggunakan tabs. Pastikan pengaturan ini pada Xcode.
* Metode tanda kurung dan tanda kurung lain (`if`/`else`/`switch`/`while` fll.) HARUS dibuka pada baris yang sama dengan pernyataaan. Tanda kurung harus ditutup pada baris baru.

**Contohnya:**
```objc
if (user.isHappy) {
    // Do something
}
else {
    // Do something else
}
```

* HARUS ada 1 baris kosong antara metode untuk mempermudah pengorganisasian dan memperjelas tampilan kode.
* Spasi pada metode MUNGKIN memisahkan kegunaan, walaupun hal ini mengindikasikan kesempatan untuk memisahkan metode menjadi beberapa metode yang lebih kecil. Pada metode dengan nama yang panjang dan rumit, spasi satu baris DAPAT digunakan untuk membantu bantuan visual dengan memisahkan badan metode.
* `@synthesize` dan `@dynamic` HARUS masing-masing dideklarasikan pada baris baru.

## Conditionals

Badan conditional HARUS menggunakan tanda kurung walaupun dapat ditulis tanpa menggunakannya untuk menghidari [errors](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). Salah satu errornya adalah menambahkan baris kedua dan mengharapkan bahwa baris tersebut menjadi bagian dari pernyataan if. Error lainnya  yang lebih berbahaya](http://programmers.stackexchange.com/a/16530) dapat terjadi ketika baris "didalam" pernyataan if dimaksudkan sebagai komentar, dan baris selanjutnya tanpa disengaja menjadi bagian dari pernyataan if. Ditambah lagi gaya ini lebih konsisten terhadap conditional lain sehingga lebih mudah dibaca.

**Contohnya:**
```objc
if (!error) {
    return success;
}
```

**Bukan:**
```objc
if (!error)
    return success;
```

atau

```objc
if (!error) return success;
```

### Ternary Operator

Tujuan dari operator ternary, `?` , adalah untuk menambah kejelasan dan kerapian kode. Ternary HANYA boleh untuk mengevaluasi 1 kondisi untuk setiap pernyataan. Evaluasi banyak kondisi biasanya lebih mudah dipahami sebagai pernyataan if atau diubah menjadi variable yang diberi nama.

**Contohnya:**
```objc
result = a > b ? x : y;
```

**Bukan:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Error Handling

Jika suatu metode mengembalikan parameter error by reference, kode HARUS berganti ke nilai kembalian dan BUKAN berganti ke variable yang error.

**Contohnya:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}
```

**Bukan:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Beberapa dari API Apple membuat nilai sampah (garbage value) pada parameter error (if non-NULL) jika status sukses, sehingga ketika terjadi error dapat menyebabkan false negatives (atau bahkan crash).

## Metode

Pada metode, HARUS ada spasi setelah scope(`-` or `+` symbol) dan HARUS ada spasi antar bagian metode.

**Contohnya:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## Variabel

Variabel HARUS diberikan nama yang jelas dan deskriptif sesuai dengan penggunaan variabel tersebut. Hal ini ditujukan untuk mempermudah programmer untuk menggunakan variable tersebut secar baik dan benar.

**Contohnya:**

* `NSString *title`: kita secara logis dapat berasumsi bahwa "title" adalah string.
* `NSString *titleHTML`: Penamaan ini menunjukan bahwa judul kemungkinan berisi HTML yang harus di-_parsing_ untuk ditampilkan. _"HTML" diperlukan oleh programmer untuk menggunakan variabel ini secara efektif_
* `NSAttributedString *titleAttributedString`: Judul yang sudah diatur untuk dutampilkan. _`AttributedString` menunjukan bahwa nilai ini hanyalah judul biasa, dan menambahkannya bisa jadi pilhan yang baik tergantung konteks._
* `NSDate *now`: _Sudah jelas dan tidak perlu penjelasan_
* `NSDate *lastModifiedDate`: Penggunaan `lastModified` dapat menjadi ambigu pada konteks tertentu, orang dapat berasumsi berebda-beda.
* `NSURL *URL` vs. `NSString *URLString`: PAda situasi dimana nilai dapat direpresentasikan oleh berbagai kelas, terkadang lebih baik untuk menghilangkan ambiguitas dari penamaan variabel.
* `NSString *releaseDateString`: Contoh lain dimana nilai dapat direpresentasikan oleh kelas lain dan penamaan dapat membantu menghilangkan ambiguitas.

Penamaan menggunakan 1 huruf TIDAK DIREKOMENDASIKAN kecuali untuk penghitungan variabel pada perulangan.

Asterisks(*) mengindikasikan tipe pointer yang HARUS merujuk ke nama variabel **Contohnya,** `NSString *text` **bukan** `NSString* text` atau `NSString * text`, kecuali juka variabel merupakan konstanta (`NSString * const NYTConstantString`).

Definisi properti HARUS diletakan secara terpisah jika dimungkinakn . Akses terhadap direct instance variable  HARUS dihindari kecuali pada initializer metode (`init`, `initWithCoder:`, etc…), methode `dealloc` dan pada custom getter atau setter. Untuk informasi lebih lanjut, silahkan buka [Dokumentasi Apple pada penggunaan accessor methods pada initializer methods dan `dealloc`](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Contoh:**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Bukan:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

#### Variable Qualifiers

Mengenai variable qualifiers [diperkenalkan oleh ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), qualifier (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) HARUS be diletakan di antara asterisks dan nama variabel, contohnya: `NSString * __weak text`.

## Penamaan

Konvensi penamaan Apple HARUS ditaati sebisa mungkin terutama pada kasus yang berhubungan dengan [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Nama variable yang panjang namun deskriptif lebih baik daripada yang singkat namun kurang deskriptif.

**Contohnya:**

```objc
UIButton *settingsButton;
```

**Bukan**

```objc
UIButton *setBut;
```

Imbuhan 3 huruf (contohnya: `NYT`) HARUS digunakan pada nama kelas dan konstanta, namun DAPAT di kecualikan untuk nama entitas Core Data. Konstanta Harus menggunakan camel-case dengan semua huruf kapital dan diberi imbuhan sesuai dengan nama kelas untu memperjelas. Imbuhan 2 huruf (contohnya: `NS`)  [sudah digunakan oleh Apple](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/DefiningClasses/DefiningClasses.html#//apple_ref/doc/uid/TP40011210-CH3-SW12).

**Contohnya:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Bukan:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Properti dan variabel lokal HARUS menggunakan _camel-case_ dengan huruf pertama menggunakan huruf kecil.

Instance variables HARUS menggunakan _camel-case_ dengan huruf pertama menggunakan huruf kecil dana menggunakan imbuhan garis bawah. Hal ini sesuai dengan instance variable yang dihasilkan oleh LLVM. **Jika LLVM dapat menghasilkan variabel secara otomatis, biarkan LLVM melakukannya.**

**Contohnya:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Bukan:**

```objc
id varnm;
```

### Kategori

Kategori DIREKOMENDASIKAN untuk memperjelas pembagian fungsionalitan dan harus diberi nama untuk mendeskripsikan fungsi tersebut.

**Contohnya:**

```objc
@interface UIViewController (NYTMediaPlaying)
@interface NSString (NSStringEncodingDetection)
```

**Bukan:**

```objc
@interface NYTAdvertisement (private)
@interface NSString (NYTAdditions)
```

Metode dan properti yang ditambahkan pada Kategori HARUS diberi nama dengan imbuhan app- atau organization-specific. Hal ini ditujukan untuk menghindari overriding terhadap fungsi yang sudah ada dan mengurangi kemungkinan 2 kategori dari pustaka (_library_) yang berbeda untuk menambahkan metode dengan nama yang sama. (Runtime Objective-C tidak menspesifikasikan metode mana yang akan dipanggil pada kasus teresbut sehingga dapat menyebabkan efek yang tidak diduga)

**Contohnya:**

```objc
@interface NSArray (NYTAccessors)
- (id)nyt_objectOrNilAtIndex:(NSUInteger)index;
@end
```

**Bukan:**

```objc
@interface NSArray (NYTAccessors)
- (id)objectOrNilAtIndex:(NSUInteger)index;
@end
```

## Komentar

Ketika dibutuhkan, komentar harus digunakan untuk menjelaskan **Bagaimana** bagian kode tersebut melakukan sesuatu. Setiap komentar yang digunakan HARUS selalu dipebarui atau dihapus.

Komentar block TIDAK DIREKOMENDASIKAN, hal ini bertujuan agar sebuah kode dapat menjadi dokumentasi bagi dirinya sendiri dengan baris penjelasan sesedikit mungkin. HAl ini tidak berlaku pada komentar yang digunakan untuk menghasilkan dokumentasi.

## init and dealloc

Metode `dealloc` HARUS diletakan di atas implementasi, setelah pernyataan `@synthesize` dan `@dynamic`. Metode `init` HARUS diletakan langsung di bawah metode `dealloc` pada setiap kelas.

Metode `init` harus disusun sebagai berikut:

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

Literal `NSString`, `NSDictionary`, `NSArray`, dan `NSNumber` HARUS  digunakan setiap kali membuat immutable instances dari objek terebut. Perhatikan dengan seksama nilai `nil` tidak dilempar ke literal `NSArray` dan `NSDictionary`, karena dapat menyebabkan _crash_.

**Contohnya:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Bukan:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## Fungsi `CGRect`

Ketika mengakses `x`, `y`, `width`, atau `height` dari `CGRect`, kode HARUS menggunakan [Fungsi-fungsi `CGGeometry`](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) dan bukan mengakses secara langsung anggota struct. Dari `CGGeometry` referensi oleh Apple:

> Semua fungsi yang dideskripsikan pada referensi ini yang mengambil struktur data CGRect sebagai masukan secara implisit menstandarisasi segitiga tersebut sebelum menhitung hasilnya. Oleh karena itu, aplikasi Anda harus menghindari pembacaan dan penulisan secara langsung pada struktur data yang tersimpan pada CGRect. Sebagai gantinya, gunakn fungsi yang dideskripsikan disini untuk memanipulasi segitiga dan mengambil karakteristiknya.

**Contohnya:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Bukan:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Konstanta

Konstanta lebih DIREKOMENDASIKAN daripada in-line string literal atau numbers. Konstanta HARUS dideklarasikan sebagai konstanta `static`. KOnstanta dapat dideklarasikan sebagai `#define` ketika secara eksplisit digunakan sebagai macro.

**Contohnya:**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**Bukan:**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## Tipe Enumerated

Ketika menggunakan `enum`, HARUS digunakan spesifikasi tipe fixed underlying yang baru ; hal ini menyajikan pengecekan tipe yang dan pelengkapan kode lebih baik. SDK mengandung macro untuk memfasilitasi penggunaan tipe fixed underlying: `NS_ENUM()`.

**Contohnya:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## Bitmasks

Ketika menggunakan bitmasks, macro `NS_OPTIONS` HARUS digunakan.

**Contohnya:**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
    NYTAdCategoryAutos      = 1 << 0,
    NYTAdCategoryJobs       = 1 << 1,
    NYTAdCategoryRealState  = 1 << 2,
    NYTAdCategoryTechnology = 1 << 3
};
```

## Private Properties

Private properties BOLEH dideklarasikan pada ekstensi kelas (kategori anonim) pada implementasi kelas file.

**Contohnya:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Pemberian nama gambar

Nama gambar harus diberikan secara konsisten untuk menjaga keteraturan dan kesehatan jiwa developer. Gambar HARUS diberi nama menggunakan _camel-case_ yang mendiskripsikan kegunaannya, diikuti dengan nama tanpa prefix dari kelas atau properti yang mengaturnya (jika ada), diikuti dengan deskripsi yang lebih detail mengenai waran dan/atau penempatan dan terakhir kodisinya

**Contohnya:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Gamba ryang digunakan untu tujuan serupa HARUS dikelompokan dalam masing-masing grup pada folder Images atau Asset Catalog.

## Booleans

Suatu nilai TIDAK BOLEH dibandingkan secara langsung terhadap `YES`, karena `YES` is defined as `1`, dan `BOOL` pada Objective-C merupaka tipe `CHAR` dengan panjang 8 bit (sehingga nilai `11111110` akan mengembalikan `NO` jika dibandingkan dengan `YES`).

**Untuk pointer objek:**

```objc
if (!someObject) {
}

if (someObject == nil) {
}
```

**For a `BOOL` value:**

```objc
if (isAwesome)
if (!someNumber.boolValue)
if (someNumber.boolValue == NO)
```

**Bukan:**

```objc
if (isAwesome == YES) // Never do this.
```

Jika properti `BOOL` diberi nama menggunakan kata sifat, BOLEH dihilangkan imbuhan `is`, namun harus sesuai dengan nama konvensional pada getter.

**Contohnya:**

```objc
@property (assign, getter=isEditable) BOOL editable;
```

_Teks dan contoh diambil dari [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)._

## Singletons

Objek singleton HARUS menggunakan pola thread-safe untuk membuat shared instance.
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
Hal ini akan mencegah [kemungkinan dan seringnya crash](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Imports

Jika ada lebih dari 1 pernyataan import, maka pernyataan-pernyataan tersebut HARUS dikelompokan [together](http://ashfurrow.com/blog/structuring-modern-objective-c). Groups BOLEH diberikan komentar.

Catatan: Untuk modul gunakan syntax [@import](http://clang.llvm.org/docs/Modules.html#using-modules).

```objc
// Frameworks
@import QuartzCore;

// Models
#import "NYTUser.h"

// Views
#import "NYTButton.h"
#import "NYTUserView.h"
```

## Protokol

Pada [protokol delegate atau data source ](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/DelegatesandDataSources/DelegatesandDataSources.html), parameter pertama pada setiap metode HARUS merupakan objek yang akan mengirim pesan.

Hal ini membantu pada kasus ketika sebuah objek didelegasikan pada banyak objek dengan tipe yang mirip yaitu dengan membantu mengklarifikasi maksud dari kelas yang mengimplementasikan metode delegate kepada pembaca.

**Contohnya:**

```objc
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath;
```

**Bukan:**

```objc
- (void)didSelectTableRowAtIndexPath:(NSIndexPath *)indexPath;
```

## Xcode project

File fisik HARUS disinkronisasikan dengan file pada proyek Xcode untuk menghidari _file sprawl_. Setiap grup Xcode yang dibuat HARUS direfleksikan oleh folder pada _filesystem_. Kode HARUS dikelompokan tidak hanya berdasarkan tipe, melainkan juga berdasarkan fitur untuk lebih memperjelas.

Target Build Setting “Treat Warnings as Errors” HARUS diaktifkan. Aktifkan sebanyak mungkin [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings). Jika ingin mengabaikan peringatan tertentu gunakan [Clang’s pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Panduan Objective-C Lain

Jika panduan kami kurang sesuai dengan selera Anda, Anda dapat melihat panduan berikut:

* [Google](https://google.github.io/styleguide/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-style-guide)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
* [Wikimedia](https://www.mediawiki.org/wiki/Wikimedia_Apps/Team/iOS/ObjectiveCStyleGuide)
