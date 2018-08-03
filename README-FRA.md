# NYTimes - Guide de Style Objective-C

Ce guide décrit les conventions de codage des équipes iOS du New York Times. Nous vous remercions pour vos commentaires dans les [tickets](https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls) et [tweets](https://twitter.com/nytimesmobile). Aussi, [nous recrutons](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY-10001/73366300/).

Merci à tous [nos contributeurs et contributrices](https://github.com/NYTimes/objective-c-style-guide/contributors).

## Introduction

Voici quelques-uns des documents d'Apple qui nous ont servi à écrire ce guide. Si quelque chose n'est pas mentionné ici, il est probablement couvert en détail dans&#8239;:

* [Le langage de Programmation Objective-C](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
* [Les Bases Fondamentales de Cocoa](https://developer.apple.com/legacy/library/documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Conseils Généraux de Codage pour Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [Guide de Programmation pour App iOS](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table des matières

* [Notation pointée](#notation-pointée)
* [Espacement](#espacement)
* [Conditions](#conditions)
  * [Opérateur ternaire](#opérateur-ternaire)
* [Gestion des erreurs](#gestion-des-erreurs)
* [Méthodes](#méthodes)
* [Variables](#variables)
* [Nommage](#nommage)
* [Commentaires](#commentaires)
* [Init & Dealloc](#init-et-dealloc)
* [Libellés](#libellés)
* [Fonctions CGRect](#fonctions-cgrect)
* [Constantes](#constantes)
* [Types énumérés](#types-énumérés)
* [Masques de bits](#masques-de-bits)
* [Propriétés privées](#propriétés-privées)
* [Nommage d'image](#nommage-dimage)
* [Booléens](#booléens)
* [Singletons](#singletons)
* [Imports](#imports)
* [Projet Xcode](#projet-xcode)

## Notation pointée

La notation pointée doit **toujours** être utilisée pour lire ou modifier les propriétés. La notation crochée est préférable dans tous les autres cas.

**Par exemple:**
```objc
view.backgroundColor = UIColor.orangeColor;
UIApplication.sharedApplication.delegate;
```

**Non pas:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
[UIApplication sharedApplication].delegate;
```

## Espacement

* L'indentation est de 4 espaces. N'indentez jamais avec des tabulations. Assurez-vous de régler cette préférence dans Xcode.
* L'accolade ouvrante des méthodes et structures de contrôle (`if`/`else`/`switch`/`while` etc.) est toujours sur la même ligne que la déclaration et l'accolade fermante sur sa propre ligne. 

**Par exemple:**
```objc
if (utilisateur.estHeureux) {
    //Faire quelque chose
}
else {
    //Faire quelque chose d'autre
}
```
* Les méthodes devraient être séparées par une ligne blanche pour améliorer la lisibilité et l'organisation. À l'intérieur des méthodes, des sauts de lignes devraient séparer les sections logiques, mais souvent ces dernières devraient être placées dans de nouvelles méthodes.
* `@synthesize` et `@dynamic` devraient chacun être déclarés sur de nouvelles lignes dans l'implémentation.

## Conditions

Les instructions de condition doivent toujours utiliser des accolades même quand la condition pourrait être écrite sans (par ex. sur une seule ligne) pour éviter des [erreurs](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). Une de ces erreurs serait d'ajouter une deuxième ligne et de penser qu'elle fait partie de la condition. Une autre, [plus dangereuse](http://programmers.stackexchange.com/a/16530) peut arriver quand la ligne «&#8239;intérieure&#8239;» de la condition est commentée, et la prochaine ligne devient involontairement une partie de la condition. De plus, ce style est plus cohérent avec d'autres conditions et donc plus facile à détecter.

**Par exemple:**
```objc
if (!error) {
return success;
}
```

**Non pas:**
```objc
if (!error)
return success;
```

ou

```objc
if (!error) return success;
```

### Opérateur ternaire

L'opérateur ternaire, `?` , doit seulement être utilisé s'il rend le code plus lisible ou propre. Il doit seulement évaluer une condition simple. Évaluer plusieurs conditions est généralement plus facile à comprendre avec une condition de type if, ou refactorisé avec des variables nommées.

**Par exemple:**
```objc
result = a > b ? x : y;
```

**Non pas:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Gestion des erreurs

Quand une méthode renvoie un paramètre d'erreur par référence, continuez l'exécution du programme sur la valeur returnée, et non sur la variable erreur.

**Par exemple:**
```objc
NSError *error;
if (![self FaireQuelqueChoseAvecErreur:&error]) {
// Gérer l'erreur
}
```

**Non pas:**
```objc
NSError *error;
[self FaireQuelqueChoseAvecErreur:&error];
if (error) {
// Gérer l'erreur
}
```

Certaines APIs d'Apple renvoient des valeurs de données poubeille pour un paramètre erreur (si non-NULL), donc continuer l'exécution du programme sur l'erreur peut créer des faux négatifs (et par la suite un plantage).

## Méthodes

Pour la signature d'une méthode, il doit y avoir un espace après le scope (symbole `-` ou `+`). Et il doit y avoir un espace entre les différents segments (paramètres) de la méthode.

**Par exemple**:
```objc
- (void)setTextePourExemple:(NSString *)texte image:(UIImage *)image;
```
## Variables

Les variables doivent être nommées de la façon la plus descriptive possible. Une variable d'une seule lettre doit être évitée sauf pour une boucle `for`.

Les astérisques qui indiquent le pointeur sont plaçés avant le nom de la variable, par ex., `NSString *text` et non `NSString* text` ou `NSString * text`, sauf dans le cas de constantes (`NSString * const NYTConstantString`).

La définition des propriétés doivent être utilisées à la place des variables d'instance quand c'est possible. L'accès direct aux variables d'instance doit être évité sauf pour les méthodes d'initialisation (`init`, `initWithCoder:`, etc…), la méthode `dealloc` et les accesseurs et mutateurs. Pour plus d'information sur l'utilisation de méthodes d'accès, les méthodes d'initialisation et dealloc, consultez [cet article](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Par exemple:**

```objc
@interface NYTSection : NSObject

@property (nonatomic) NSString *headline;

@end
```

**Non pas:**

```objc
@interface NYTSection : NSObject {
NSString *headline;s
}
```

#### Qualification des variables

En ce qui concerne les qualificateurs de variables [introduits avec ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), le qualificateur (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) doit être placé entre l'astérisque et le nom de la variable, par ex., `NSString * __weak text`.

## Nommage

La convention de nommage Apple devrait être suivie quand possible, surtout en ce qui concerne les [règles de management de mémoire](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Il est mieux d'utiliser des noms descriptifs, et longs si nécessaire, pour les méthodes et variables.

**Par exemple:**

```objc
UIButton *settingsButton;
```

**Non pas**

```objc
UIButton *setBut;
```

Un préfixe de trois lettres (par ex. `NYT`) doit toujours être utilisé pour le nom des classes et constantes, mais peut être omis pour le nom des entités dans Core Data. Les constantes doivent adopter la convention camelCase avec tous les mots qui commencent avec une lettre capitale, précédées du nom de la classe dans laquelle ils sont déclarés pour la clarté.

**Par exemple:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Non pas:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Les properties et variables locales doivent adopter la convention camelCase avec le premier mot en minuscules.

Les variables d'instance doivent adopter la convention camelCase avec le premier mot en miniscules, précédé par le préfixe  «&#8239;_&#8239;». Ceci est cohérent avec les variables d'instance synthetisées automatiquement par LLVM. **Si LLVM peut synthetiser la variable automatiquement, laissez-le faire.**

**Par exemple:**

```objc
@synthesize nomDeVariableDescriptif = _nomDeVariableDescriptif;
```

**Non pas:**

```objc
id nmvar;
```

## Commentaires

Si nécessaire, les commentaires peuvent être utilisés pour expliquer **pourquoi** un bloc de code fait quelque chose. Les commentaires doivent être à jour ou éliminés.

Les paragraphes de commentaires devraient généralement être évités, le code devrait être suffisament descriptif, avec seulement un besoin intermittent de commentaire et juste quelques lignes d'explications. Ceci ne s'applique pas aux commentaires utilisés pour la documentation.

## init et dealloc

La méthode `dealloc` doit être placée au début de l'implémentation, directement après les expressions `@synthesize` et `@dynamic`. La méthode `init` doit être placée directement après la méthode `dealloc`.

La méthode `init` doit être structurée comme ceci:

```objc
- (instancetype)init {
self = [super init]; // ou appeler l'initialisateur désigné
if (self) {
// Initialisation particulière à cet object
}
return self;
}
```

## Libellés

`NSString`, `NSDictionary`, `NSArray`, et `NSNumber` literals doivent être utilisés quand des instances immutables sont créées pour ces objets. Faites bien attention que la valeur `nil` ne soit pas passée aux literals `NSArray` et `NSDictionary`, parce que ça causerait un plantage.

**Par exemple:**

```objc
NSArray *names = @[@"Brian", @"Craig", @"Véronique"];
NSDictionary *productManagers = @{@"iOS": @"Andrew", @"Android": @"Kate"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Non pas:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Craig", @"Véronique", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Andrew", @"iOS", @"Kate", @"Android", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## Fonctions `CGRect`

En accédant à `x`, `y`, `width`, ou `height` d'un `CGRect`, utilisez toujours les [fonctions `CGGeometry`](https://developer.apple.com/documentation/coregraphics/cggeometry) au lieu de l'accès direct au membre struct. Extrait de la référence Apple pour `CGGeometry`:

> Toutes les fonctions décrites dans cette référence qui prendre les structures de data CGRect comme donnée standardise implicitement ces rectangles avant de calculer leurs résultats. Pour cette raison, votre application devrait éviter de lire et écrire directement la donnée sauvegardée dans la structure de données CGRect. À la place, utilisez les fonctions décrites ici pour manipuler les rectangles et pour recupérer leurs caractériques.

**Par exemple:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Non pas:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constantes

Les constantes sont préférables aux literals in-line ou aux nombres, parce qu'elles peuvent être facilement reproduites de variables utilisés souvent et parce qu'elles peuvent être changées facilement sans avoir besoin de faire une recherche. Constantes devraient être déclarées avec `static` et non pas `#define`s à moins qu'elle soient utilisées explicitement comme macro.

**Par exemple:**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**Non pas:**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## Types énumérés

Pour l'utilisation d' `enum`, il est recommendé de choisir le type fixe spécifié avec un «&#8239;_&#8239;» parce qu'il est de type fort et pour bénéficier de la complétion de code. Le SDK inclus maintenant un macro pour faciliter et encourager l'utilisation de type fixe et souligné — `NS_ENUM()`

**Exemple:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
NYTAdRequestStateInactive,
NYTAdRequestStateLoading
};
```

## Masques de bits

Quand vous travaillez avec des masques de bits, utilisez le macro `NS_OPTIONS`.

**Exemple:**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
NYTAdCategoryAutos      = 1 << 0,
NYTAdCategoryJobs       = 1 << 1,
NYTAdCategoryRealState  = 1 << 2,
NYTAdCategoryTechnology = 1 << 3
};
```

## Propriétés privées

Les propriétés privées doivent être déclarées dans l'extension de la classe dans le fichier d'implémentation.

**Par exemple:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic) GADBannerView *googleAdView;
@property (nonatomic) ADBannerView *iAdView;
@property (nonatomic) UIWebView *adXWebView;

@end
```

## Nommage d'image

Les noms des images doit être cohérents pour préserver une bonne organisation. Elles doivent être nommées en utilisant la convention camelCase avec la description de leur utilisation, suivi du suffixe de la classe ou propriété qu'elles customisent (si elle existe), suivie de la description de la couleur et/ou emplacement, et finalement leur état.
**Par exemple:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarBlanc` / `ArticleNavigationBarBlanc@2x` and `ArticleNavigationBarNoirSelected` / `ArticleNavigationBarNoirSelected@2x`.

Les images qui sont utilisées à des fins similaires doivent être regroupées dans leurs groupes respectifs à l'intérieur d'un dossier Images ou d'un «&#8239;Asset Catalog&#8239;».


## Booléens

Puisque `nil` est retourné comme `NO` il n'est pas nécessaire de le comparer dans une condition. Ne comparez jamais quelque chose avec `YES`, parce que `YES` est défini comme 1 et un `BOOL` peut aller jusqu'à 8 bits.

Ce style permet une plus grande cohérence entre les différents fichiers et une meilleure clarté visuelle.

**Par exemple:**

```objc
if (!unObject) {
}
```

**Non pas:**

```objc
if (unObject == nil) {
}
```

-----

**Pour un `BOOL`, voici deux exemples:**

```objc
if (estSuper)
if (!unObject.boolValue)
```

**Non pas:**

```objc
if (estSuper == YES) // Ne faites pas ça
if (unObject.boolValue == NO)
```

-----

Si le nom d'une propriété `BOOL` est exprimée comme un adjectif, la propriété peut omettre le prefixe «&#8239;is&#8239;» mais doit specifier un nom conventionel pour l'accesseur get, par exemple:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Voyez le document et exemple pris de [Conseils Généraux de Nommage Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Les singletons doivent utiliser un patron thread-safe (état de processus cohérent sans engendrer de problèmes de concurrence) pour créer leur instance unique et partagé.

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
Celui permet d'éviter des [plantages possibles, et parfois fréquents](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Imports

S'il y a plusieurs directives d'importation, divisez-les en [groupes](http://ashfurrow.com/blog/structuring-modern-objective-c).
Commenter chaque groupe est facultatif.

Note&#8239;: pour les modules utilisez la syntaxe [@import](http://clang.llvm.org/docs/Modules.html#using-modules).

```objc
// Frameworks
@import QuartzCore;

// Models
#import "NYTUser.h"

// Views
#import "NYTButton.h"
#import "NYTUserView.h"
```

## Projet Xcode

Les fichiers physiques doivent être maintenus en accordance avec le projet Xcode pour éviter d'avoir des fichiers éparpillés. Les groupes crées dans Xcode doivent avoir un dossier équivalent dans le système de fichiers. Le code doit être groupé non seulement par type, mais aussi par caractéristique pour une plus grande clarté.

Si possible, choisissez toujours «&#8239;Treat Warnings as Errors&#8239;» dans le Build Settings du target et exposer autant d'[avertissements supplémentaires](http://boredzo.org/blog/archives/2009-11-07/warnings) que possible. Si vous avez besoin d'ignorer un avertissement specifique, utiliser [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Autres guides de style Objective-C

Si le nôtre n'est pas à votre goût, consultez ces autres guides:

* [Google](https://google.github.io/styleguide/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
