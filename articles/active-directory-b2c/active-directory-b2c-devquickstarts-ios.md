<properties
    pageTitle="Azure Active Directory B2C: Üçüncü taraf kitaplıklar kullanılarak bir iOS uygulamasından web API'si çağırma | Microsoft Azure"
    description="Bu makale size OAuth 2.0 taşıyıcı belirteçlerini kullanarak Node.js web API'si çağıran iOS 'yapılacaklar listesi' uygulamasının bir üçüncü taraf kitaplık kullanılarak nasıl oluşturulacağını gösterir"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags ms.service="active-directory-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic="hero-article"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# Azure AD B2C: Üçüncü taraf kitaplık kullanılarak bir iOS uygulamasından web API’si çağırma

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Microsoft kimlik platformu OAuth2 ve OpenID Connect gibi açık standartlar kullanır. Bunun yapılması geliştiricilerin hizmetlerimizle tümleştirmek istediği herhangi bir kitaplıktan yararlanmasını sağlar. Geliştiricilerin platformumuzu diğer kitaplıklarla birlikte kullanmasına yardımcı olmak amacıyla, üçüncü taraf kitaplıkların Microsoft identity platformuna bağlanmak üzere nasıl yapılandırılacağını gösteren bunun gibi birkaç izlenecek yol oluşturduk. [RFC6749 OAuth2 belirtimi](https://tools.ietf.org/html/rfc6749) uygulayan çoğu kitaplık Microsoft Identity platformuna bağlanabilecektir.


OAuth2 veya OpenID Connect kullanmaya yeni başladıysanız bu örnek yapılandırmanın büyük bölümü sizin için çok anlamlı olmayabilir. [burada belgelediğimiz protokolün genel bakışına](active-directory-b2c-reference-protocols.md) kısaca bakmanız önerilir.

> [AZURE.NOTE]
    Platformumuzun Koşullu Erişim ve Intune ilke yönetimi gibi bu standartlarda bir ifadeye sahip olmayan bazı özellikleri, açık kaynak Microsoft Azure Kimlik Kitaplıkları kullanmanızı gerektirir. 
   
B2C platformu tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez.  B2C platformunu kullanmanızın gerekli olup olmadığını belirlemek için [B2C sınırlamaları](active-directory-b2c-limitations.md) hakkında bilgi edinin.


## Azure AD B2C dizini alma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## Uygulama oluşturma

Ardından B2C dizininizde uygulama oluşturmanız gerekir. Bu, uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Bu durumda, tek bir mantıksal uygulama içerdikleri için hem uygulama hem de web API'si tek bir **Uygulama Kimliği** ile temsil edilir. Bir uygulama oluşturmak için [bu talimatları](active-directory-b2c-app-registration.md) izleyin. Şunları yaptığınızdan emin olun:

- Uygulamaya bir **mobil cihaz** ekleyin.
- Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Buna da daha sonra ihtiyacınız olacak.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## İlkelerinizi oluşturma

Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu uygulama bir kimlik deneyimi içerir: oturum açma ve kaydolma birleşimi. Her tür için [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy) tanımlanan şekilde bu ilkeyi oluşturmanız gerekir. İlkeyi oluştururken şunları yaptığınızdan emin olun:

- İlkenizde **Görünen ad** ve kaydolma özniteliklerini seçin.
- Her ilkede uygulamanın talep ettiği **Görünen ad** ve **Nesne Kimliği** öğelerini seçin. Diğer talepleri de seçebilirsiniz.
- Oluşturduktan sonra her ilkenin **Adını** kaydedin. `b2c_1_` önekine sahip olmalıdır.  Bu ilke adına daha sonra ihtiyacınız olacaktır.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

İlkelerinizi oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.


## Kodu indirme

Bu öğretici için kod [GitHub'da](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c) korunur.  Takip etmek için [uygulamayı .zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/arşiv/master.zip olarak indirin) veya kopyalayın:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Veya yalnızca tamamlanan kodu indirin ve hemen başlayın: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## nxoauth2 üçüncü taraf kitaplığını indirin ve bir çalışma alanı başlatın

Bu kılavuzda GitHub’da, Mac OS X ve iOS (Cocoa ve Cocoa touch) için bir OAuth2 kitaplığı olan OAuth2Client’ı kullanacağız. Bu kitaplık OAuth2 belirtimi taslak 10’u temel alır. Yerel uygulama profilini uygular ve son kullanıcı yetkilendirme uç noktasını destekler. Microsoft identity platformu ile tümleştirme için başka bir şey gerekli değildir.

### Kitaplığınızı CocoaPods kullanarak projenize ekleme

CocoaPods, Xcode projeleri için bir bağımlılık yöneticisidir. Yukarıdaki yükleme adımlarını otomatik olarak yönetir.

```
$ vi Podfile
```
Aşağıdakileri bu podfile dosyaya ekleyin:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Şimdi cocoapods kullanarak podfile dosyasını yükleyin. Bunun yapılması yükleyeceğiniz yeni bir XCode Çalışma Alanı oluşturur.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## Projenin yapısı

Projemizin iskeletinde aşağıdaki yapı ayarlanmıştır:

* **Ana Görünüm** ve bir görev bölmesi
* Seçili görevin verileri için **Görev Ekleme Görünümü**
* Kullanıcının uygulamada oturum açmasına imkan tanıyan **Oturum Açma Görünümü**.

Kimlik doğrulaması eklemek için projedeki çeşitli dosyalara atlanacaktır. Kodun görsel kod gibi diğer kısımları kimlikle ilgili değildir ve sizin için belirtilir.

## Uygulamanız için `settings.plist` dosyasını oluşturma

Yapılandırma değerlerinin yerleştirileceği merkezi bir konum mevcutsa uygulamanın yapılandırılması daha kolaydır. Ayrıca her bir ayarın uygulamanızda neler yaptığını anlamanıza yardımcı olur. Bu değerleri uygulamaya sağlamanın bir yolu olarak *Özellik Listesi*’nden yararlanılacaktır.

* Uygulama çalışma alanınızdaki `Supporting Files` altında `settings.plist` dosyası oluşturun/açın

* Aşağıdaki değerleri girin (bunları daha sonra ayrıntılı olarak ele alınacaktır)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Şimdi bunlara ayrıntılı olarak bakalım.


`authURL`, `loginURL`, `bhh`, `tokenURL` için kiracı adınızı doldurmanız gerektiğini fark edeceksiniz. Bu kiracı adı, size atanan B2C kiracınızın adıdır. Örneğin, `kidventusb2c.onmicrosoft.com`. Açık kaynak Microsoft Azure Kimlik Kitaplıklarını kullanırsanız meta veri uç noktası kullanılarak bu veriler sizin için çekilir. Bu değerleri sizin yerinize ayıklamak için çok çalıştık.

B2C kiracı adları hakkında daha fazla bilgi için şuraya göz atın: [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)

`keychain` değeri, NXOAuth2Client kitaplığının belirteçlerinizi depolamak üzere bir anahtarlık oluştururken kullanacağı kapsayıcıdır. Çok sayıda uygulamada SSO edinmek istiyorsanız uygulamalarınızın her birinde aynı anahtarlığı belirtebilir ve bu anahtarlığın XCode yetkilendirmelerinizde kullanılmasını isteyebilirsiniz. Bu konu Apple belgelerinde ele alınmıştır.

Her URL’nin sonundaki `<policy name>`, yukarıda oluşturduğunuz ilkeyi koyduğunuz yerlerdir. Uygulama, akışa bağlı olarak bu ilkeleri çağıracaktır.

`taskAPI`, görev eklemek ya da var olan görevleri sorgulamak üzere B2C belirtecinizle birlikte çağrılacak REST Uç Noktasıdır. Bu ayar bu örnek için özel olarak oluşturulmuştur. Örneğin çalışması için değiştirmeniz gerekmez.

Bu değerlerin geri kalanı kitaplığı kullanmak için gereklidir ve değerleri bağlama taşımak üzere sizin için yerler oluşturur.

`settings.plist` dosyası oluşturulmuştur, okunması için kod gereklidir.

## Ayarları okumak için bir AppData sınıfı oluşturun

Yukarıda oluşturulan `settngs.plist` dosyasını ayrıştıran basit bir dosya oluşturalım ve bu ayarları gelecekte herhangi bir sınıfta kullanılabilir hale getirelim. Bir sınıf tarafından her istendiğinde verilerin yeni bir kopyasını oluşturmak istemediğimizden bir Singleton deseni kullanılacak ve ayarlar için istek yapılan her durumda yalnızca oluşturulan aynı örnek döndürülecektir

* Bir `AppData.h` dosyası oluşturun:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Bir `AppData.m` dosyası oluşturun:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Bundan böyle aşağıda göreceğiniz sınıfların herhangi birinde yalnızca `  AppData *data = [AppData getInstance];` çağrısı yaparak verilere ulaşılabilir.



## AppDelegate içinde NXOAuth2Client kitaplığınızı ayarlayın

NXOAuthClient kitaplığı bazı değerlerin ayarlanmasını gerektirir. Bu işlem tamamlandıktan sonra REST API’sini çağırmak için elde edilen belirteci kullanabilirsiniz. `AppDelegate` öğesinin uygulamayı yüklediğimiz herhangi bir zaman çağrılacağı bilindiğinden yapılandırma değerlerinin bu dosyaya yerleştirilmesi mantıklıdır.
* `AppDelegate.m` dosyasını açın

* Daha sonra kullanacağımız bazı başlık dosyalarını içeri aktarın.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* AppDelegate’e `setupOAuth2AccountStore` yöntemini ekleyin

Bir AccountStore oluşturulması ve ardından `settings.plist` dosyasından okunan verilerle beslenmesi gerekir.

Bu noktada B2C hizmetiyle ilgili olarak bu kodu daha anlaşılır hale getirecek bazı bilgileri öğrenmeniz gerekmektedir:


1. Azure AD B2C, isteğinize hizmet vermek üzere sorgu parametreleri tarafından sağlanan *ilkeyi* kullanır. Bunun yapılması Azure Active Directory’nin yalnızca uygulamanız için bağımsız bir hizmet olarak görev yapmasını sağlar. Bu ek sorgu parametrelerini sağlamak için özel ilke parametrelerimizle birlikte `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` yöntemini sağlamamız gerekir. 

2. Azure AD B2C, diğer OAuth2 sunucularıyla çok benzer şekilde kapsamları kullanır. Ancak, B2C kaynaklara erişim kadar kullanıcı kimliğini doğrulamayla da ilgili olduğundan, akışın doğru çalışması için bazı kapsamlar kesinlikle gereklidir. Bu, `openid` kapsamıdır. Microsoft identity SDK’ları `openid` kapsamını size otomatik olarak sağlar, bu nedenle SDK yapılandırmasında görmezsiniz. Ancak, bir üçüncü taraf kitaplığı kullandığımız için bu kapsamı belirtmemiz gerekir.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Ardından, bu çağrıyı `didFinishLaunchingWithOptions:` yöntemi altındaki AppDelegate içinde yaptığınızdan emin olun. 

```
[self setupOAuth2AccountStore];
```


## Kimlik doğrulama isteklerini işlemek için kullanılacak bir `LoginViewController` sınıfı oluşturun

Hesapta oturum açmak için bir web görünümü kullanılır. Bunun yapılması kullanıcıdan SMS metin iletisi (yapılandırılmışsa) gibi ek faktörler istememize veya kullanıcıya hata iletileri göndermemize imkan tanır. Burada web görünümü ayarlanacak ve ardından Microsoft Identity Service içindeki WebView’de gerçekleşecek geri çağırmaları işlemek üzere kod yazılacaktır.

* Bir `LoginViewController.h` sınıfı oluşturun

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Bu yöntemlerin her biri aşağıda oluşturulacaktır.

> [AZURE.NOTE] 
    `loginView` öğesini film şeridinizin içindeki gerçek web görünümüne bağladığınızdan emin olun. Aksi takdirde kimlik doğrulaması gerektiğinde açılan bir web görünümünüz olmaz.

* Bir `LoginViewController.m` sınıfı oluşturun

* Kimlik doğrulaması yapılırken taşıma durumuna bazı değişkenler ekleyin

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Kimlik doğrulamasını işlemek için WebView yöntemlerini geçersiz kılın

Bir kullanıcının yukarıda açıklanan şekilde oturum açması gerektiğinde istenen davranışın web görünümüne anlatılması gerekir. Aşağıdaki kodu kesip yapıştırmanız yeterlidir.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* OAuth2 isteğinin sonucunu işlemek için kod yazın

WebView’dan geri gelen redirectURL’yi işleyecek kod gereklidir. Başarısız olmadıysa yeniden denenecektir. Bu sırada kitaplık, konsolda görebileceğiniz veya zaman uyumsuz olarak işleyebileceğiniz hatayı sağlar. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Bildirim oluşturucuları ayarlayın.

Yukarıdaki `AppDelegate` adımında oluşturulan yöntemin aynısı oluşturulur, ancak bu kez hizmetimizde neler olduğunu söylemesi için bazı `NSNotification` öğeleri eklenecektir. Belirteçle birlikte herhangi bir şey değiştiğinde bize bildiren bir gözlemci ayarlanır. Belirteci aldıktan sonra kullanıcı `masterView` hedefine geri gönderilir.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* sign-native için bir istek başlatılan her durumda kullanıcıyı işleyen kodu ekleyin

Kimlik doğrulama isteği aldığımız her durumda çağrılacak bir yöntem oluşturalım. Bu yöntem gerçek anlamda bir web görünümü oluşturacaktır

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Son olarak, yukarıda yazılan tüm bu yöntemleri `LoginViewController` her yüklendiğinde çağıralım. Yöntemleri Apple’ın sağladığı `viewDidLoad` yöntemine ekleyerek bunu yaparız

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Oturum açma için uygulamamızla etkileşim kurmanın temel yolunu oluşturdunuz. Oturum açtıktan sonra, aldığımız belirteçleri kullanmanız gerekir. Bunun için bu kitaplığı kullanarak REST API'lerini çağıracak bazı yardımcı kodlar oluşturulacaktır.


## Bir REST API’sine gönderdiğimiz istekleri işleyen bir `GraphAPICaller` sınıfı oluşturun

Uygulamamızı her yüklediğimizde yüklenen bir yapılandırmamız var. Artık bir belirtecimiz olduğuna göre bununla bir şeyler yapmamız gerekiyor. 

* Bir `GraphAPICaller.h` dosyası oluşturun

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Bu koddan iki yöntem oluşturulacağını görebilirsiniz: bir API’den görevleri almak için bir tane ve API’ye görev eklemek için bir tane.

Arabirimimizi ayarladığımıza göre gerçek uygulamayı ekleyelim:

* Oluşturun: `GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## Örnek uygulamayı çalıştırma

Son olarak Xcode'da uygulamayı oluşturun ve çalıştırın. Uygulamaya kaydolun veya uygulamada oturum açın ve oturum açan bir kullanıcı için görevler oluşturun. Oturumu kapatın, farklı bir kullanıcı olarak yeniden oturum açın ve o kullanıcı için görevler oluşturun.

API, aldığı erişim belirtecinden kullanıcının kimliğini ayıkladığı için görevlerin API üzerinde kullanıcı başına depolanmasına dikkat edin.


## Sonraki adımlar

Artık daha ileri seviyeli B2C konu başlıklarına geçebilirsiniz: Deneyebilecekleriniz:

[Node.js web uygulamasından Node.js web API'si çağırma]()

[B2C uygulaması için kullanıcı deneyimini özelleştirme]()



<!--HONumber=Aug16_HO1-->


