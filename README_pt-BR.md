# NYTimes • Convenção de código Objective-C

Este guia descreve as convenções de desenvolvimento da equipe de iOS do The New York Times.
Sua contribução é bem-vinda em nossas [issues](https://github.com/NYTimes/objetive-c-style-guide/issues), [pull requests](https://github.com/NYTimes/objetive-c-style-guide/pulls) e [tweets](https://twitter.com/nytimesmobile).
Além disso, [estamos contratando](http://jobs.nytco.com/job/New-York-iOS-Developer-Job-NY/2572221/).

Agradecemos a todos os [nossos contribuidores](https://github.com/NYTimes/objective-c-style-guide/contributors).

## Introdução

Seguem alguns dos documentos oficiais da Apple que informam a convenção padrão da linguagem.
Caso algo não seja mencionado aqui, as seguintes referências podem ajudar:

* [A linguagem de programação Objective-C](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Guia de fundamentos Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
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
  * [Underscores](#underscores)
* [Comentários](#comentários)
* [Init e Dealloc](#init-e-dealloc)
* [Literais](#literais)
* [Funções CGRect](#funções-cgrect)
* [Constantes](#constantes)
* [Tipos enumerados](#tipos-enumerados)
* [Propriedades privadas](#propriedades-privadas)
* [Nomenclatura de imagens](#nomenclatura-de-imagens)
* [Booleanos](#booleanos)
* [Singletons](#singletons)
* [Projeto no Xcode](#projeto-no-Xcode)

## Quando utilizar ponto

Utilize o ponto **sempre** quando for acessar e alterar uma propriedade. Em todos os outros casos é recomendada a utilização dos colchetes.

**Exemplo correto:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Inadequado:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
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
* Deve haver exatamente uma linha em branco entre os métodos para auxiliar na organização visual. Espaços em branco dentro dos métodos devem separar funcionalidades, mas sua principal função é separadar métodos.
* `@synthesize` e `@dynamic` devem ser declarados em novas linhas.

## Condicionais

Condicionais, como por exemplo `if`, devem sempre utilizar chaves (mesmo em casos onde o corpo da condicional necessite apenas de uma linha) evitando assim esse tipo de [erro](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). Esses erros incluem a adição de uma segunda linha e esperam que a mesma faça parte do bloco. Outra [falha](http://programmers.stackexchange.com/a/16530) ainda maior pode ocorrer quando a linha que faz parte da condicional é comentada fazendo com que a próxima linha, involuntariamente, se torne parte da condicional. Além disso, esse estilo é mais consistente com todas as outras condicionais e, portanto, mais facilmente interpretado.

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

O operador ternário, `?`, deve ser utilizado apenas em caso onde sua aplicação facilita a interpretação e clareza visual do código. Ele deve ser aplicado quando uma condição é avaliada. A avaliação de várias condições é mais compreensível com uma instrução `if`.

**Exemplo correto:**
```objc
result = a > b ? x : y;
```

**Inadequado:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Tratamento de erros

Quando os métodos retornarem um parâmetro de erro referênciado, deve-se tratar o valor retornado e não a variável de erro.

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

Algumas APIs da Apple armazenam valores ao parâmetro de erro sem existir um erro de fato (um exemplo seria armazenar o valor 'não existem erros'), por isso validar se a váriavel é nula pode ocasionar a falso negativo (e, posteriormente, falhas).

## Métodos

Na assinatura de um método, deve haver um espaço após o escopo (símbolo de -/+). Também deve existir um espaço entre os parâmetros dos métodos.

**Examplo correto:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```
## Variáveis

As variáveis devem ser nomeadas de forma mais descritiva possível. Nomes de variáveis com uma única letra devem ser evitados, exceto em estruturas de repetição do tipo `for()`.

Os asteriscos, que indicam ponteiros, pertencem a variável, por exemplo: `NSString *text`, não `NSString* text` ou `NSString * text`, exceto em caso onde sejam aplicados a constantes.

As definições de propriedade vem ser utilizadas sempre que possível. O acesso direto a variáveis de instância deve ser evitado, com excessão de métodos inicializadores (`init`, `initWithCoder:`...), métodos `dealloc` e métodos acessores (`set` e `get`). Para mais informações sobre o uso de métodos acessores em métodos inicializadores e `dealloc`, consulte [esta página](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Exemplo correto:**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Inadequado:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

## Nomenclaturas

As convenções de nomenclatura da Apple devem ser respeitadas sempre que possível, especialmente aquelas relacionadas a [regras de gerenciamento de memória](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Nomes longos para variáveis e métodos são uma boa opção.

**Exemplo correto:**

```objc
UIButton *settingsButton;
```

**Inadequado:**

```objc
UIButton *setBut;
```

O prefixo de 3 letras (exemplo: `NYT`) deve sempre ser usado em nomes de classes e contantes, no entanto, pode ser omitido para nomes de entidades do tipo Core Data. Contantes devem ser camel-case com todas as palavras em a maiúsculo e prefixadas com o nome da classe, facilitando a interpretação do código.

**Exemplo correto:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Inadequado:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Propriedades devem ser nomeadas utilizando o padrão camel-case com a primeira palavra em letras minúsculas. **Se o Xcode sintetiza a variável automaticamente, utilize esse recurso.** Caso contrário, para manter a consistência, a variável de instância referenciada nessa propriedade deve utilizar o padrão camel-case iniciando com o underscore (`_`) e a primeira palavra com letras minúsculas. Este é o formato de síntese padrão do Xcode.

**Exemplo correto:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Inadequado:**

```objc
id varnm;
```

### Underscores

Quando uma propriedade for utilizada, as variáveis de instância devem ser acessadas ou alteradas com o `self.`, isso significa distinguir as propriedades visualmente. Variáveis locais não devem conter underscores.

## Comentários

Quando forem necessários, os comentários devem explicar o porquê um determinado bloco de código faz algo. Qualquer comentário utilizado deve ser mantido atualizado ou excluído.

Blocos de comentários devem ser evitados, assim como o código deve se auto-documentar, ou seja, necessitar de uma quantidade mínima de explicações. Isso não se aplica as informações utilizadas para gerar uma documentação.

## init e dealloc

Métodos `dealloc` devem er colocados no topo das classes de implementação, logo após as declarações de `@synthesize` e `@dynamic`. O `init` deve ser colocado imediatamente abaixo do método `dealloc` de qualquer classe.

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

`NSString`, `NSDictionary`, `NSArray` e `NSNumber` devem ser usados sempre para a criação de instâncias imutáveis desses objetos. Atenção especial a utilização do valor `nil` em `NSArray` e/ou `NSDictionary`, ela pode gerar falha.

**Exemplo correto:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
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

Ao acessar `x`, `y`, `width` ou `height` de um `CGRect`, sempre use as [funções `CGGeometry`](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) ao invés de acessar os membros diretamente. Referência da Apple sobre as funções `CGGeometry`:

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

Constantes têm preferência sobre strings literais ou números, uma vez que permitem fácil reprodução de variáveis comumente usadas e podem ser rapidamente alteradas sem a necessidade de localizar e substituir. Constantes devem ser declaradas como `static` e não `#define`, a menos que sejam utilizadas explicitamente como macros.

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

Ao usar `enum`s, recomenda-se utilizar a nova espeficicação de tipo fixo subjacente pois este tem forte verificação de código. O SDK inclui agora uma macro que facilita e incentiva o uso de tipos fixos subjacentes - `NZ_ENUM()`.

**Example:**

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## Propriedades privadas

Propriedades privadas devem ser decladas em extensões da classe (categorias anônimas) no arquivo de implementação. Categorias nomeadas (como `NYTPrivate` ou `private`) jamais devem ser utilizadas, a menos que sejam extensões de outra classe.

**Exemplo correto:**

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Nomenclatura de imagens

O nome de uma imagem deve ser consistente, preservando a organização e o objetivo ao qual foi criada. Ela deve utilizar o padrão camel-case para descrever sua finalidade, seguido do prefixo da classe ou propriedade que esta sendo personalizada (caso exista), seguido por uma descrição mais detalhada de sua coloração e, finalmente, seu estado (selecionado, por exemplo).

**Exemplo correto:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Imagens que são utilizadas para um propósito similar devem fazer parte do mesmo grupo, dentro de uma pasta.

## Booleanos

`nil` é interpretado como `NO` portanto não é necessário compara-lo em condições. Nunca compare algo diretamente com `YES` porque `YES` é definido como 1 e um `BOOL` pode ser de até 8 bits.

Isso permite uma maior consistência entre os arquivos e maior clareza visual.

**Exemplo correto:**

```objc
if (!someObject) {
}
```

**Inadequado:**

```objc
if (someObject == nil) {
}
```

-----

**Para um `BOOL`, temos dois exemplos:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**Inadequado:**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Never do this.
```

-----

Se o nome de uma propriedade do tipo `BOOL` é expresso como um adjetivo, a propriedade pode omitir o prefixo "is", mas deve continuar especificando o nome convencional para o metodo de acesso `get`. Exemplo:

```objc
@property (assign, getter=isEditable) BOOL editable;
```

Textos e exemplos tirados do [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

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

## Projeto no Xcode

Os arquivos físicos (estrutura de diretórios do Finder), devem ser mantidos em sincronia com os arquivos do projeto no Xcode. Qualquer grupo criado no Xcode deve refletir na geração de uma pasta no sistema de arquivos (Finder). O código não deve ser agrupado apenas pelo tipo, mas também pela sua característica comum.

Quando possível, sempre tratar os avisos de alerta como erros no `target Build Settings` e habilitar todos os tipos de [alertas adicionais](http://boredzo.org/blog/archives/2009-11-07/warnings) possíveis. Caso seja necessário ignorar uma advertência específica, utilize a [pragma do Clang](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Outras convenções de código Objective-C

Caso não goste de nossa convenção, segue abaixo outros padrões:

* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
