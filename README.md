## The New York Times Objective–C Style Guide

This style guide outlines the coding conventions of the iOS team at The New York Times. We welcome your feedback in [issues](https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls), or [tweets](https://twitter.com/nytimesmobile).

## Introduction

Here are some of the documents that went into building the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Dot-Notation Syntax](#dot-notation-syntax)
* [Spacing](#spacing)
* [Conditionals](#conditionals)
* [Methods](#methods)
* [Variables](#variables)
* [Naming](#naming)
  * [Underscores](#underscores)
* [Init & Dealloc](#init-and-dealloc)
* [Literals](#literals)
* [CGRect Functions](#cgrect-functions)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Private Properties](#private-properties)
* [Image Naming](#image-naming)
* [Booleans](#booleans)
* [Singletons](#singletons)
* [Executing Code After a Delay](#executing-code-after-a-delay)
* [Concurrency](#concurrency)
* [Notifications](#notifications)
* [Xcode Project](#xcode-project)

## Dot-Notation Syntax

Dot syntax notation should always be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**For example:**  
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
[singer setName:@"Taylor" age:22];
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.

**For example:**  
```objc
if (condition) {
//Do something
}
else {
//Do something else
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (i.e., it is one line only) braces should still be used. There are just too many little ways to get burned otherwise.

**For example:**
```objc
if (condition) {
    return success;
}
```

**Not:**
```objc
if (condition)
    return success;
```

or  

```objc
if (condition) return success;
```

## Methods

In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments.  

**For Example**:  
```objc  
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```
## Variables

Variables should be named as descriptively as possible. Single letter variables should be avoided except in `for()` loops. 

Asterisks indicating pointers belong with the variable, i.e. `NSString *text` not `NSString* text` or `NSString * text`.

Property definitions should be used in place of naked instance variables. As of Xcode 3.2.5, we should no longer be declaring any instance variables at all. Direct instance variable access should be avoided except in `dealloc` methods and within custom setters and getters.

**For example:**  

```objc
@property (nonatomic, retain) NSString *newspaperTitle;
```

**Not:**

```objc
NSString *newspaperTitle;
```

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to memory management rules (NARC). 

Long, descriptive method and variable names are good. 

**For example:**  

```objc
UIButton *settingsButton;
```

**Not**  

```
UIButton *setBut;
```

The `NYT` prefix should always be used for classes and constants. Constants should be camel-case with all words capitalized and prefixed by the class name for clarity. 

**For example:**  

```objc
const NSTimeInterval NYTArticleViewControllerFadeAnimationDuration = 0.3;
```

**Not:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties should be camel-case with the leading word being lowercase. **If Xcode can automatically synthesize the variable, then let it.** Otherwise, in order to be consistent, the backing instance variables for these properties should be camel-case with the leading word being lowercase and a leading underscore. This is the same format as Xcode's default synthesis.

**For example:**  

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**

```objc
id varnm;
```

### Underscores

When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`. 

As such, and in anticipation of the move to ARC, the need for underscores in local variables is far diminished. It serves only to complicate and slow down typing and efficiency and propagate an unnecessarily different naming convention. **Therefore, local variables should not contain underscores.**

## Comments

When they are needed, comments should be used to explain why a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

## init and dealloc

`dealloc` methods should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `init` should be placed directly below the `dealloc` methods of any class.

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that nil values not be passed into `NSArray` and `NSDictionary` literals, as this will now cause a crash. Previously, the list would be nil-terminated early and produce unexpected behavior.

**For example:**  

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"}; //note that the format is now `key : value` as opposed to the old method of `value, key`
NSNumber *shouldUseLiterals = @YES;
NSNumber *zipCode = @10018;
```

**Not:**  

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *zipCode = [NSNumber numberWithInteger:10018];
```

### CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access.

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

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

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables, and can be quickly and dynamically changed. Constants should be declared as static constants and not `#define`s unless explicitly being used as a macro. 

**For example:**  

```objc
static NSString * const NYTConstantName = @"Name";  

static const CGFloat NYTConstantSize = 10.0;
```

**Not:**  

```objc
	#define ConstantName @"Name"
	
	#define ConstantSize 2
```

## Enumerated Types

When using enums, it is recommended to use the new fixed underlying type specification. This looks like

```objc
typedef enum : NSInteger {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading    
} NYTAdRequestState;
```

This has the following important advantages:
1. Better code completion
2. Stronger type checking

The SDK now includes a macro to facilitate and encourage use of fixed underlying types -- `NS_ENUM()`

**Example:**  

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## Private Properties

Private variables should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `NYTPrivate` or `private`) should never be used unless extending another class.

**For example:**  

```objc
@interface NYTAdvertisement ()

@property (nonatomic, retain) GADBannerView *googleAdView;
@property (nonatomic, retain) ADBannerView *iAdView;
@property (nonatomic, retain) UIWebView *adXWebView;

@end
```

## Image Naming

Image names should be named consistently to preserve organization and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state. 

**For example:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`. 

Images that are used for a similar purpose should be grouped in respective groups in an Images folder.

## Booleans

Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits. 

This allows for more consistency across files and greater visual clarity. 

**For example:**  

```objc
if (!someObject){
}
```
		
**Not:**  

```objc
if (someObject == nil){
}
```

-----

**For a BOOL, here's two examples:**  

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

If the name of a BOOL property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+ (id)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
   sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes] (http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Executing Code After a Delay

`dispatch_after` is preferred over `performSelector:withDelay:` for executing code after a delay, because if `self` is deallocated before `performSelector:withDelay:` executes, a crash will occur. `dispatch_after` however, retains self until the block is finished executing. NOTE: To avoid retain cycles do not hold a strong reference to the block. [Further reading about performSelector crashes](http://adamernst.com/post/10052965047/be-careful-with-performselector).

## Concurrency

Explicitly managing threads with `NSThread` is expressly discouraged. As of iOS 4, there are much better and more performant ways of taking advantage of multi-threaded programming - in particular Grand Central Dispatch. Please refer to Apple's document on [Migrating Away From Threads](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/ConcurrencyProgrammingGuide/ThreadMigration/ThreadMigration.html) for reference. Since manually managing threads is strongly discouraged, so are locking mechanisms such as `@synchronized`. 

When attempting to protect a portion of code with locking, consider the root cause - why is a variable being mutated or mutating code being executed at the same time across threads? Additionally, `@synchronized` is a slow lock that wastes valuable CPU cycles. Further, nesting `@synchronized` does not make it any more effective. If a lock is absolutely necessary, consider instead using `dispatch_async` on a private serial queue. 

## Notifications

Notifications, while useful, are often unnecessary. Before creating a notification, carefully consider how many objects need to be notified of an event. Often, the delegation pattern or blocks are more appropriate.

Block-based notification observation is to be avoided as it can easily cause inadvertent retain cycles and there are issues with removing observers from within blocks. See [Ben Scheirman's article](http://benscheirman.com/2012/01/careful-with-block-based-notification-handlers) for further reference.

## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.
