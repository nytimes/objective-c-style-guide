# NYTimes - Guide de Style Objective-C

Ce guide présente les conventions de codage de l'équique iOS au New York Times. Nous vous remercions de vos commentaires pour les [problèmes](https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls) et [tweets](https://twitter.com/nytimesmobile). Aussi, [Nous recrutons](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY-10001/73366300/).

Merci à tous [nos collaborateurs (trices)](https://github.com/NYTimes/objective-c-style-guide/contributors).

## Introduction

Voici une partie de la documentation Apple qui nous ont aidé à écrire ce guide. Si quelque chose n'est pas mentionné ici, c'est parce que c'est certainement couvert en détail dans:

* [Le Language de Programmation Objective-C](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Les Bases Fundamentales de Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Conseils Générals de Codage pour Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [Guide de Programmation pour App iOS](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table des Matières

* [La Notation Pointée](#notation-pointée)
* [Espacement](#espacement)
* [Conditions](#conditions)
* [Opérateur Ternaire](#operateur-ternaire)
* [Gestion d'Erreurs](#gestion-d'erreurs)
* [Méthodes](#méthodes)
* [Variables](#variables)
* [Nommage](#nommage)
* [Commentaires](#commentaires)
* [Init & Dealloc](#init-et-dealloc)
* [Literals](#literals)
* [Fonctions CGRect](#fonctions-cgrect)
* [Constantes](#constantes)
* [Type Enumération](#type-enumeration)
* [Bitmasks](#bitmasks) // Masque Binaire
* [Propriétés Privées](#propriétés-privées)
* [Nommage d'Image](#nommage-image)
* [Booléens](#booléens)
* [Singletons](#singletons)
* [Project Xcode](#project-xcode)

## La Notation Pointée

La notation pointée doit **toujours** être utilisée pour accéder aux ou changer les propriétés. La notation crochée est préférable dans tous les autres cas.

**Par exemple:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Non pas:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Espacement

* L'indentation est de 4 espaces. N'utilisez jamais la touche de tabulation pour l'indentation. Assurez-vous de choisir cette préférence dans Xcode.
* Les accolades des méthodes et autres accolades (`if`/`else`/`switch`/`while` etc.) d'ouverture sont toujours sur la même ligne que l'instruction mais les accolades de fermeture sont sur une autre ligne.

**Par exemple:**
```objc
if (utilisateur.estHeureux) {
//Faire quelque chose
}
else {
//Faire quelque chose d'autre
}
```
* It doit y avoir exactement une ligne vide entre les méthodes pour clarité visuelle et meilleure organisation. Une ligne vide à l'intérieur d'une méthode indique une séparation de fonction, qui devrait souvent être mise dans une nouvelle méthode.
* `@synthesize` et `@dynamic` doivent chacun être déclarés sur une nouvelles ligne dans l'implementation.

## Conditions

Les instructions de condition doivent toujours utiliser des accolades même quand la condition pourrait être écrite sans (par ex. sur une seule ligne) pour éviter des [erreurs](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). Une des ces erreurs serait d'ajouter une deuxième ligne et penser qu'elle fait partie de la condition. Une autre, [plus dangeureuse](http://programmers.stackexchange.com/a/16530) peut arriver quand la ligne "intérieure" de la condition est commentée, et la prochaine ligne devient involontairement une partie de la condition. De plus, ce style is plus cohérent avec d'autres conditions et donc plus facile à détecter.

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

or

```objc
if (!error) return success;
```

### Opérateur Ternaire

L'opérateur ternaire, ? , doit seulement être utilisé s'il rend le code plus lisible ou propre. Il ne doit seulement évaluer une condition simple. Evaluer plusieurs conditions est généralement plus facile à comprendre avec une condition de type if, ou refactoré avec des variables d'instance.

**Par exemple:**
```objc
result = a > b ? x : y;
```

**Non pas:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Gestion d'Erreurs

Quand une méthode renvoie un paramètre erreur par référence, continuez l'exécution du programme sur la value returnée, et non sur la variable erreur.

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

Certains des APIs d'Apple renvoient des valeurs de données poubeille pour un paramètre erreur (si non-NULL), donc continuer l'exécution du programme sur l'erreur peut créer des faux négatifs (et par la suite un plantage).

## Méthodes

Pour la signature d'une méthode, il doit y avoir un espace après le scope (symbole -/+). Et il doit y avoir un espace entre les différent segments (paramètres) de la méthode.

**Par exemple**:
```objc
- (void)setTextePourExemple:(NSString *)texte image:(UIImage *)image;
```
## Variables

Les variables doivent être nommées de la façon la plus descriptive possible. Une variable d'une seule lettre doit être évitée sauf pour une boucle `for()`.

Les astérisques qui indiquent le pointeur sont plaçés avant le nom de la variable, par ex., `NSString *text` et non `NSString* text` ou `NSString * text`, sauf dans le cas de constantes.

La définition des propriétés doivent être utilisées à la place des variables d'instance quand c'est possible. L'accès direct aux variables d'instance doit être évité sauf pour les méthodes d'initialisation (`init`, `initWithCoder:`, etc…), la méthode `dealloc` et les setters et getters. Pour plus d'information sur l'utilisation de méthodes d'accès, les méthodes d'initialisation et dealloc, consultez [cet article](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Par exemple:**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Non pas:**

```objc
@interface NYTSection : NSObject {
NSString *headline;s
}
```

#### Qualifiers de Variables

En ce qui concerne les qualifiers de variables [introduits avec ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), le qualifier (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) doit être placé entre l'astérisque et le nom de la variable, par ex., `NSString * __weak text`.

## Nommage

La convention de nommage Apple devrait être suivi quand possible, surtout en ce qui concerne les [règles de management de mémoire](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Il est mieux d'utiliser des noms descriptifs, et longs si nécessaire, pour les méthodes et variables.

**Par exemple:**

```objc
UIButton *settingsButton;
```

**Non pas**

```objc
UIButton *setBut;
```

Un préfixe de trois lettres (par ex. `NYT`) doit toujours être utilisé pour le nom des classes et constantes, mais peut être omit pour le nom des entités dans Core Data. Les constantes doivent adopter la convention camelCase avec tous les mots qui commençent avec une lettre capitale, précédés du nom de la classe dans laquelle ils sont déclarés pour clarité.

**Par exemple:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Non pas:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Les properties et local variables doivent adopter la convention camelCase avec le premier mot en miniscules.

Les variables d'instance doivent adopter la convention camelCase avec le premier mot en miniscules, précédé par le préfixe "_". Ceci est cohérent avec les variables d'instance synthetisé automatiquement par LLVM. **Si LLVM peut synthetiser la variable automatiquement, laissez-le faire.**

**Par exemple:**

```objc
@synthesize nomDeVariableDescriptif = _nomDeVariableDescriptif;
```

**Non pas:**

```objc
id nmvar;
```

## Commentaires

Si nécessaire, les commentaires peuvent être utilisés pour expliquer **pourquoi** un bloc de code fait quelque chose. Les commentaires doivrent être à jour ou éliminés.

Les paragraphes de commentaires devraient généralement être évités, le code devrait être suffisament descriptif, avec seulement un besoin intermittent de commentaire et juste quelques lignes d'explanation. Ceci ne s'applique pas aux commentaires utilisés pour la documentation.

## init et dealloc

La méthode `dealloc` doit être plaçée au début de l'implementation, directement après les expressions `@synthesize` et `@dynamic`. La méthode `init` doit être plaçée directement après la méthode `dealloc`.

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

## Literals

`NSString`, `NSDictionary`, `NSArray`, et `NSNumber` literals doivent être utilisés quand des instances immutables sont crées pour ces objects. Faites bien attention que la value `nil` ne soit pas passée aux literals `NSArray` et `NSDictionary`, parce que ça causerait un plantage.

**Par exemple:**

```objc
NSArray *names = @[@"Brian", @"Craig", @"Véronique"];
NSDictionary *productManagers = @{@"iOS" : @"Andrew", @"Android" : @"Kate"};
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

## Fonctions CGRect

Quand accéder au `x`, `y`, `width`, ou `height` d'un `CGRect`, utiliser toujours les [fonctions `CGGeometry`](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) au lieu de l'accès directe au membre struct. Extrait de la référence Apple pour `CGGeometry`:

> Toutes les fonctions décrites dans cette référence qui prendre les structures de data CGRect comme donnée standardise implicitement ces rectangles avant de calculer leurs résultats. Pour cette raison, votre application devrait éviter de lire et écrire directement la donnée sauvegardée dans la structure de data CGRect. A la place, utiliser les fonctions décrites ici pour manipuler les rectangles et pour recupérer leurs charactériques.

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

## Type Enumération

Pour l'utilisation d' `enum`, il est recommendé de choisir le type fixe spécifié avec un "_" parce qu'il est de type fort et pour bénéficier de la complétion de code. Le SDK inclus maintenant un macro pour faciliter et encourager l'utilisation de type fixe et souligné — `NS_ENUM()`

**Exemple:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
NYTAdRequestStateInactive,
NYTAdRequestStateLoading
};
```

## Bitmasks

Quand vous travaillez avec des bitmasks, utilisez le macro `NS_OPTIONS`.

**Exemple:**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
NYTAdCategoryAutos      = 1 << 0,
NYTAdCategoryJobs       = 1 << 1,
NYTAdCategoryRealState  = 1 << 2,
NYTAdCategoryTechnology = 1 << 3
};
```

## Propriétés Privées

Les propriétés privées doivent être déclarées dans l'extension de la classe dans le fichier d'implementation. Des catégories appelées (telles que `NYTPrivate` ou `private`) ne doivent jamais être utilisées à moins qu'elle soit extendues d'une autre classe.

**Par exemple:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic) GADBannerView *googleAdView;
@property (nonatomic) ADBannerView *iAdView;
@property (nonatomic) UIWebView *adXWebView;

@end
```

## Nommage d'Image

Les noms des images doit être cohérent pour préserver une bonne organisation. Elles doivent être nommées en utilisant la convention camelCase avec la description de leur utilisation, suivi du suffixe de la classe ou propriété qu'elles customisent (si elle existe), suivie de la description de la couleur et/ou emplacement, et finalement leur état.
**Par exemple:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarBlanc` / `ArticleNavigationBarBlanc@2x` and `ArticleNavigationBarNoirSelected` / `ArticleNavigationBarNoirSelected@2x`.

Les images qui sont utilisées pour un même goal doivent être groupées dans leur groupe respectif à l'intérieur d'un dossier Images.


## Booléens

Puisque `nil` est retourné comme `NO` il n'est pas nécessaire de le comparer dans une condition. Ne comparez jamais quelque chose avec `YES`, parce que `YES` est défini comme 1 et un `BOOL` peut aller jusqu'à 8 bits.

Ce style permet une plus grande cohérence entre les différent fichiers et une meilleure clarité visuelle.

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
if (![unObject boolValue])
```

**Non pas:**

```objc
if (estSuper == YES) // Ne faites pas ça
if ([unObject boolValue] == NO)
```

-----

Si le nom d'une propriété `BOOL` est exprimée comme un adjectif, la propriété peut omettre le prefix “is” mais doit specifier un nom conventional pour l'accesseur get, par exemple:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Voyez le document et exemple pris de [Conseils Générals de Nommage Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Les singletons doivent utiliser un patron thread-safe (état de processus cohérent sans engendrer de problèmes de concurrence) pour créer leur instance unique et partagé.

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
Celui permet d'éviter des [plantages possibles, et parfois fréquentes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Project Xcode

Les fichiers physiques doivent être maintenus en accordance avec le project Xcode pour éviter d'avoir des fichiers éparpillés. Les groupes crées dans Xcode doivent avoir un dossier équivalent dans le système de fichiers. Le code doit être groupé non seulement par type, mais aussi par charactéristique pour une plus grande clarité.

Si possible, choisissez toujours "Treat Warnings as Errors" dans le Build Settings du target et exposer autant d'[avertissements supplémentaires](http://boredzo.org/blog/archives/2009-11-07/warnings) que possible. Si vous avez besoin d'ignorer un avertissement specifique, utiliser [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Autres guides de style Objective-C

Si le notre n'est pas à votre goût, consultez ces autres guides:

* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
