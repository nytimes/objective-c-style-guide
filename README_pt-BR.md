# NYTimes • Convenção de código Objective-C

Este guia descreve as convenções de desenvolvimento da equipe de iOS do The New York Times.
Sua contribução é bem-vinda em nossas [issues](https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls) e [tweets](https://twitter.com/nytimesmobile).
Além disso, [estamos contratando](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY/2572221/).

Agradecemos a todos os [nossos contribuidores](https://github.com/NYTimes/objective-c-style-guide/contributors).

## Introdução

Seguem alguns dos documentos oficiais da Apple que informam a convenção padrão da linguagem.
Caso algo não seja mencionado aqui, as seguintes referências podem ajudar:

* [A linguagem de programação Objective-C](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
* [Guia de fundamentos Cocoa](https://developer.apple.com/legacy/library/documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Convenções de código Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [Guia de programação de aplicativos iOS](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Índice

* [Quando utilizar ponto](#quando-utilizar-ponto)
* [Espaçamento](#espaçamento)
* [Condicionais](#condicionais)
  * [Operador ternário](#operador-ternário)
* [Tratamento de erros](#tratamento-de-erros)
* [Métodos](#métodos)
* [Variáveis](#variáveis)
* [Nomenclaturas](#nomenclaturas)
  * [Categorias](#categorias)
* [Comentários](#comentários)
* [Init e Dealloc](#init-e-dealloc)
* [Literais](#literais)
* [Funções CGRect](#funções-cgrect)
* [Constantes](#constantes)
* [Tipos enumerados](#tipos-enumerados)
* [Máscara de bits](#máscaras-de-bits)
* [Propriedades privadas](#propriedades-privadas)
* [Nomenclatura de imagens](#nomenclatura-de-imagens)
* [Booleanos](#booleanos)
* [Singletons](#singletons)
* [Imports](#imports)
* [Protocols](#protocols)
* [Projeto no Xcode](#projeto-no-Xcode)

## Quando utilizar ponto

Utilize o ponto **sempre** quando for acessar e alterar uma propriedade. Em todos os outros casos é recomendada a utilização dos colchetes.

**Exemplo correto:**
```objc
view.backgroundColor = UIColor.orangeColor;
UIApplication.sharedApplication.delegate;
```

**Inadequado:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
[UIApplication sharedApplication].delegate;
```

## Espaçamento

* Idente utilizando 4 espaços, Nunca idente com tabulações. Certifique-se de configurar essa preferência no Xcode.
* Métodos e qualquer bloco de códico que utilize chaves (`if`/`else`/`switch`/`while` etc.) devem sempre ser declarados na mesma linha. Feche o bloco em um nova linha.

**Exemplo correto:**
```objc
if (user.isHappy) {
// verdadeiro
}
else {
// falso
}
```
* Deve haver exatamente uma linha em branco entre os métodos para auxiliar na organização visual.
* Espaços em branco dentro dos métodos devem separar funcionalidades (embora muitas das vezes isso indique uma oportunidade para dividir o método em outros métodos menores). Em métodos com nomes longos ou verbosos, uma única linha em branco pode ser utilizada para prover separação visual antes do corpo do método.
* `@synthesize` e `@dynamic` devem ser declarados em novas linhas.

## Condicionais

Condicionais, como por exemplo `if`, devem sempre utilizar chaves (mesmo em casos onde o corpo da condicional necessite apenas de uma linha) evitando assim esse tipo de [erro](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). Esses erros incluem a adição de uma segunda linha e esperam que a mesma faça parte do bloco. Outra [falha ainda maior](http://programmers.stackexchange.com/a/16530) pode ocorrer quando a linha que faz parte da condicional é comentada fazendo com que a próxima linha, involuntariamente, se torne parte da condicional. Além disso, esse estilo é mais consistente com todas as outras condicionais e, portanto, mais facilmente interpretado.

**Exemplo correto:**
```objc
if (!error) {
    return success;
}
```

**Inadequado:**
```objc
if (!error)
    return success;
```

or

```objc
if (!error) return success;
```

### Operador ternário

O operador ternário, `?`, deve ser utilizado apenas em caso onde sua aplicação facilita a interpretação e clareza visual do código. Ele deve ser aplicado quando somente uma condição é avaliada. A avaliação de várias condições é normalmente mais compreensível com uma instrução `if`, ou refatorada em variáveis nomeadas.

**Exemplo correto:**
```objc
result = a > b ? x : y;
```

**Inadequado:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Tratamento de erros

Quando os métodos retornarem um parâmetro de erro referenciado, deve-se tratar o valor retornado e não a variável de erro.

**Exemplo correto:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // tratar o erro
}
```

**Inadequado:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // tratar o erro
}
```

Algumas APIs da Apple armazenam valores ao parâmetro de erro sem existir um erro de fato (um exemplo seria armazenar o valor 'não existem erros'), por isso validar se a váriavel é nula pode ocasionar um falso negativo (e, posteriormente, falhas).

## Métodos

Na assinatura de um método, deve haver um espaço após o escopo (símbolo `-` ou `+`). Também deve existir um espaço entre os parâmetros dos métodos.

**Examplo correto:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```
## Variáveis

As variáveis devem ser nomeadas de forma descritiva, com o nome da variável comunicando claramente o que a variável _é_ e também informações pertinentes que um programador precise para usar o valor corretamente.

**Por exemplo:**

* `NSString *title`: É sensato assumir que "title" é uma string.
* `NSString *titleHTML`: Indica que o título pode conter HTML que precisa ser tratado para ser exibido. _"HTML" é necessário para que o programador utilize essa variável efetivamente._
* `NSAttributedString *titleAttributedString`: Um título, já formatado para ser exibido. _`AttributedString` dá uma dica que esse valor não é somente um título puro, e adicioná-lo pode ser uma escolha razoável dependendo do contexto._
* `NSDate *now`: _Não é necessário especificar melhor._
* `NSDate *lastModifiedDate`: `lastModified` somente pode ser ambíguo; dependendo do contexto, uma pessoa pode assumir diferentes tipos.
* `NSURL *URL` vs. `NSString *URLString`: Em situações onde um valor pode ser representada por diferentes classes, é normalmente útil remover essa ambiguidade no nome da variável.
* `NSString *releaseDateString`: Outro exemplo onde um valor pode ser representado por outra classe, e o nome pode ajudar a remover a ambiguidade.

Variáveis com somente uma letra deve ser evitados, exceto para simples contadores em loops.

Os asteriscos, que indicam que um tipo de uma variável é um ponteiro, deve estar "anexado" ao nome da variável, **por exemplo:** `NSString *text`, **não** `NSString* text` ou `NSString * text`, exceto em caso onde sejam aplicados a constantes (`NSString * const NYTConstantString`).

As definições de propriedade devem ser utilizadas sempre que possível. O acesso direto a variáveis de instância deve ser evitado, com excessão de métodos inicializadores (`init`, `initWithCoder:`...), métodos `dealloc` e métodos acessores (`set` e `get`). Para mais informações, [veja a documentação da Apple sobre o uso de métodos acessores em métodos inicializadores e `dealloc`](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Exemplo correto:**

```objc
@interface NYTSection : NSObject

@property (nonatomic) NSString *headline;

@end
```

**Inadequado:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

### Qualificadores de variáveis

Quando se trata de qualificadores de variáveis [introduzidos com o ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), o qualificador (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) deve ser colocado entre o asterisco e o nome da variável, por exemplo: `NSString * __weak text`.

## Nomenclaturas

As convenções de nomenclatura da Apple devem ser respeitadas sempre que possível, especialmente aquelas relacionadas a [regras de gerenciamento de memória](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Nomes longos e descritivos para variáveis e métodos são uma boa opção.

**Exemplo correto:**

```objc
UIButton *settingsButton;
```

**Inadequado:**

```objc
UIButton *setBut;
```

O prefixo de 3 letras (exemplo: `NYT`) deve sempre ser usado em nomes de classes e contantes, no entanto, pode ser omitido para nomes de entidades do tipo Core Data. Contantes devem ser camel-case com todas as palavras em a maiúsculo e prefixadas com o nome da classe, facilitando a interpretação do código. Prefixo de duas letras (por exemplo: `NS`) é [reservado para uso pela Apple](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/DefiningClasses/DefiningClasses.html#//apple_ref/doc/uid/TP40011210-CH3-SW12).

**Exemplo correto:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Inadequado:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Propriedades e variáveis locais devem ser nomeadas utilizando o padrão camel-case com a primeira palavra em letra minúscula.

Variáveis de instância devem ser camel-case iniciando com a primeira palavra em letra minúscula, e devem ser prefixadas com um underscore (`_`). Esse formato é consistente com as variáveis sintetizadas automaticamente pelo LLVM. **Se o LLVM pode sintetizar uma variável automaticamente, então deixe-o.**

**Exemplo correto:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Inadequado:**

```objc
id varnm;
```

### Categorias

Categorias podem ser usadas para segmentar funcionalidade concisamente e devem ser nomeadas de modo a descrever essa funcionalidade.

**Exemplo correto:**

```objc
@interface UIViewController (NYTMediaPlaying)
@interface NSString (NSStringEncodingDetection)
```

**Inadequado:**

```objc
@interface NYTAdvertisement (private)
@interface NSString (NYTAdditions)
```

Métodos e propriedades adicionadas em categorias devem ser nomeadas com o prefixo da aplicação ou específico da organização. Isso evita sobrescrever involuntariamente um método existente, e reduz a change de duas categorias de diferentes bibliotecas adicionarem métodos com o mesmo nome. (A runtime do Objective-C não especifica qual método será chamado no último caso, o que pode levar a efeitos indesejados.)

**Exemplo correto:**

```objc
@interface NSArray (NYTAccessors)
- (id)nyt_objectOrNilAtIndex:(NSUInteger)index;
@end
```

**Inadequado:**

```objc
@interface NSArray (NYTAccessors)
- (id)objectOrNilAtIndex:(NSUInteger)index;
@end
```

## Comentários

Quando forem necessários, os comentários devem explicar o **porquê** um determinado bloco de código faz algo. Qualquer comentário utilizado deve ser mantido atualizado ou excluído.

Blocos de comentários devem ser evitados, de forma que o código deve se auto-documentar, ou seja, necessitar de uma quantidade mínima de explicações. Isso não se aplica as informações utilizadas para gerar uma documentação.

## init e dealloc

Métodos `dealloc` devem ser colocados no topo das classes de implementação, logo após as declarações de `@synthesize` e `@dynamic`. O `init` deve ser colocado imediatamente abaixo do método `dealloc` de qualquer classe.

Métodos `init` devem ser estruturados da seguinte forma:

```objc
- (instancetype)init {
    self = [super init]; // ou chame o inicializar correspondente
    if (self) {
        // Inicialização customizada
    }
    return self;
}
```

## Literais

`NSString`, `NSDictionary`, `NSArray` e `NSNumber` devem ser usados sempre para a criação de instâncias imutáveis desses objetos. Atenção especial a utilização do valor `nil` em `NSArray` ou `NSDictionary`, ela pode gerar falha.

**Exemplo correto:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Inadequado:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## Funções CGRect

Ao acessar `x`, `y`, `width` ou `height` de um `CGRect`, sempre use as [funções `CGGeometry`](https://developer.apple.com/documentation/coregraphics/cggeometry) ao invés de acessar os membros diretamente. Referência da Apple sobre as funções `CGGeometry`:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Exemplo correto:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Inadequado:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constantes

Constantes são preferidas ao invés de strings literais ou números, uma vez que permitem fácil reprodução de variáveis comumente usadas e podem ser rapidamente alteradas sem a necessidade de localizar e substituir. Constantes devem ser declaradas como `static` e não `#define`, a menos que sejam utilizadas explicitamente como macros.

**Exemplo correto:**

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**Inadequado:**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## Tipos enumerados

Ao usar `enum`s, recomenda-se utilizar a nova espeficicação de tipo fixo subjacente, pois esta tem forte verificação de código. O SDK inclui agora uma macro que facilita e incentiva o uso de tipos fixos subjacentes - `NZ_ENUM()`.

**Exemplo:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## Máscaras de bits

Quando trabalhar com máscara de bits, utilize a macro `NS_OPTIONS`.

**Exemplo:**

```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
  NYTAdCategoryAutos      = 1 << 0,
  NYTAdCategoryJobs       = 1 << 1,
  NYTAdCategoryRealState  = 1 << 2,
  NYTAdCategoryTechnology = 1 << 3
};
```

## Propriedades privadas

Propriedades privadas devem ser decladas em extensões da classe (categorias anônimas) no arquivo de implementação.

**Exemplo correto:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Nomenclatura de imagens

O nome de uma imagem deve ser consistente, preservando a organização e o objetivo ao qual foi criada. Elas deve utilizar o padrão camel-case para descrever sua finalidade, seguido do nome não prefixado da classe ou propriedade que está sendo personalizada (caso exista), seguido por uma descrição mais detalhada de sua cor e, finalmente, seu estado (selecionado, por exemplo).

**Exemplo correto:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Imagens que são utilizadas para um propósito similar devem fazer parte do mesmo grupo, dentro de uma pasta ou um `Asset Catalog`.

## Booleanos

Nunca compare algo diretamente com `YES`, porque `YES` é definido como `1`, e um `BOOL` em Objective-C é um `CHAR` que é 8 bits (então um valor `11111110` retornará `NO` se comparado com `YES`).

**Exemplo com ponteiro para objeto:**

```objc
if (!someObject) {
}

if (someObject == nil) {
}
```

**Para um valor `BOOL`:**

```objc
if (isAwesome)
if (!someObject.boolValue)
if (someObject.boolValue == NO)
```

**Inadequado:**

```objc
if (isAwesome == YES) // Nunca faça isso.
```

Se o nome de uma propriedade do tipo `BOOL` é expresso como um adjetivo, o nome da propriedade pode omitir o prefixo `is`, mas deve continuar especificando o nome convencional para o metodo de acesso `get`.

**Por exemplo:**

```objc
@property (assign, getter=isEditable) BOOL editable;
```

_Textos e exemplos tirados do [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)._

## Singletons

Objetos Singleton devem utilizar o padrão thread-safe para criação de uma instância compartilhada.
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
Isso evita [possíveis falhas](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Imports

Se existir mais de uma declaração de import, [agrupe-os](http://ashfurrow.com/blog/structuring-modern-objective-c). Comentar cada grupo é opcional.

Nota: Para módulos use a syntax [@import](http://clang.llvm.org/docs/Modules.html#using-modules).

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

Em um [protocolo de delegate ou data source](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/DelegatesandDataSources/DelegatesandDataSources.html), o primeiro parâmetro do método deve ser o objeto que envia a mensagem.

Isso ajuda a desambiguar casos onde um objeto é o delegate de múltiplos objetos de tipos similares, e isso ajuda a clarificar a intenção para os leitores de uma classe implementando esses métodos delegate.

**Exemplo correto:**

```objc
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath;
```

**Inadequado:**

```objc
- (void)didSelectTableRowAtIndexPath:(NSIndexPath *)indexPath;
```

## Projeto no Xcode

Os arquivos físicos (estrutura de diretórios do Finder), devem ser mantidos em sincronia com os arquivos do projeto no Xcode. Qualquer grupo criado no Xcode deve refletir na geração de uma pasta no sistema de arquivos (Finder). O código não deve ser agrupado apenas pelo tipo, mas também pela sua funcionalidade para maior clareza.

Quando possível, sempre habilite `Treat Warning as Errors` no `Build Settings` do `target`, para tratar os avisos de alerta como erros, e também habilite todos os tipos de [alertas adicionais](http://boredzo.org/blog/archives/2009-11-07/warnings) possíveis. Caso seja necessário ignorar uma advertência específica, utilize a [pragma do Clang](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Outras convenções de código Objective-C

Caso não goste de nossa convenção, segue abaixo outros padrões:

* [Google](https://google.github.io/styleguide/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
* [Wikimedia](https://www.mediawiki.org/wiki/Wikimedia_Apps/Team/iOS/ObjectiveCStyleGuide)
