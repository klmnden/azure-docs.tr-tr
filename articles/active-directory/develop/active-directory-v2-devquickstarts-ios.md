---
title: Oturum açma Azure AD v2.0 uç kullanarak iOS uygulama ekleme | Microsoft Docs
description: Kullanıcıların hem kişisel Microsoft hesabı ile oturum açtığında bir iOS uygulamasının nasıl oluşturulacağını ve üçüncü taraf kitaplıklar kullanılarak iş veya Okul hesapları.
services: active-directory
author: xerners
manager: mtillman
ms.assetid: fd3603c0-42f7-438c-87b5-a52d20d6344b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: 5323f9a514c3c1c6134656e41af68e479fd8fdc5
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Grafik API'si v2.0 uç noktası kullanarak bir üçüncü taraf kitaplık kullanılarak bir iOS uygulamasına oturum açma ekleme
Microsoft kimlik platformu OAuth2 ve OpenID Connect gibi açık standartlar kullanır. Geliştiricilerin hizmetlerimizle tümleştirmek istediği herhangi bir kitaplığı kullanabilirsiniz. Geliştiricilerin platformumuzu diğer kitaplıklarla birlikte kullanmak için Microsoft identity platformuna bağlanmak için üçüncü taraf kitaplıklarını yapılandırmak nasıl göstermek için bunun gibi birkaç izlenecek yazdıktan. Uygulayan çoğu kitaplık [RFC6749 OAuth2 belirtimi](https://tools.ietf.org/html/rfc6749) Microsoft identity platformuna bağlanabilir.

Bu kılavuzda oluşturan uygulamayı ile kullanıcılar kendi kuruluşa oturum açabilir ve ardından başkaları için kuruluşlarında grafik API'sini kullanarak arama.

OAuth2 veya Openıd Connect kullanmaya yeni başladıysanız Bu örnek yapılandırmanın çoğunu sizin için anlamlı değil. Okumanızı öneririz [v2.0 protokolleri - OAuth 2.0 yetkilendirme kodu akışı](active-directory-v2-protocols-oauth-code.md) arka planı için.

> [!NOTE]
> Bir ifade koşullu erişim ve Intune ilke yönetimi gibi OAuth2 veya Openıd Connect standartları olan bazı özellikleri, bizim açık kaynak Microsoft Azure kimlik kitaplıkları kullanmanızı gerektirir.
> 
> 

V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez.

> [!NOTE]
> V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

## <a name="download-code-from-github"></a>Github'dan kodu indirme
Bu öğretici için kod [GitHub'da](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2) korunur.  İzlemek için [uygulamanın çatısını bir .zip karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) veya çatıyı kopyalayın:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Örnek ayrıca indirebilirsiniz ve hemen başlayın:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Bir uygulamayı kaydetme
En yeni bir uygulama oluşturma [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya ayrıntılı adımları izleyin [v2.0 uç noktası ile bir uygulama nasıl](active-directory-v2-app-registration.md).  Emin olun:

* Kopya **uygulama kimliği** atanan uygulamanıza yakında gerekir çünkü.
* Ekleme **mobil** uygulamanız için platform.
* Kopya **yeniden yönlendirme URI'si** portalından. Varsayılan değer olan kullanmalısınız `urn:ietf:wg:oauth:2.0:oob`.

## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Üçüncü taraf NXOAuth2 kitaplık indirmeniz ve çalışma alanı oluşturma
Bu kılavuzda Github'da, Mac OS X ve iOS (Cocoa ve Cocoa touch) için bir OAuth2 kitaplığı olan OAuth2Client kullanır. Bu kitaplık OAuth2 belirtimi taslak 10’u temel alır. Yerel uygulama profilini uygular ve kullanıcı yetkilendirme uç noktası destekler. Bu Microsoft kimlik platformu ile tümleştirmeniz gerekir tüm noktalardır.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>Kitaplık CocoaPods kullanarak projenize ekleyin.
CocoaPods, Xcode projeleri için bir bağımlılık yöneticisidir. Önceki yükleme adımlarını otomatik olarak yönetir.

```
$ vi Podfile
```
1. Aşağıdakileri bu podfile dosyaya ekleyin:
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. CocoaPods kullanarak podfile dosyasını yükleyin. Bu, yükleyeceğiniz yeni bir Xcode çalışma alanı oluşturur.
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Projenin yapısı keşfedin
Projemizin iskeletinde aşağıdaki yapı ayarlandı:

* UPN arama ile ana görünüm
* Veriler için ayrıntı görünümü seçili kullanıcı hakkında
* Burada bir kullanıcı uygulamaya grafiği sorgulamak oturum açarak bir oturum açma görünümü

Biz çatıyı çeşitli dosyalarında kimlik doğrulaması ekleme taşır. Kodun görsel kod gibi diğer bölümleri kimliğine ait değil ancak sizin için sağlanır.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Kitaplığı'nda settings.plst dosya ayarlama
* Hızlı Başlangıç projeyi açın `settings.plist` dosya. Azure portalında kullanılan değerleri yansıtacak şekilde bölümdeki öğelerinin değerlerini değiştirin. Active Directory kimlik doğrulama kitaplığı kullandığında kodunuzu bu değerleri başvurur.
  * `clientId` Portalından kopyalandığından, uygulamanızın istemci kimliği.
  * `redirectUri` Portalı sağlanan yeniden yönlendirme URL'si.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>LoginViewController içinde NXOAuth2Client kitaplığınızı ayarlayın
NXOAuth2Client kitaplığı bazı değerlerin ayarlanmasını gerektirir. Bu görev tamamlandıktan sonra grafik API'sini çağırmak için alınan belirteci kullanabilirsiniz. Çünkü `LoginView` olacaktır ihtiyacımız kimliğini doğrulamak için her zaman olarak adlandırılan, bu yapılandırma değerlerinin bu dosyaya yerleştirilmesi mantıklıdır.

* Bazı değerler ekleyelim `LoginViewController.m` dosya kimlik doğrulaması ve yetkilendirme bağlamını ayarlayın. Değerleri ayrıntılarını kodu izleyin.
  
    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Kod ayrıntılarını bakalım.

İçin ilk dizedir `scopes`.  `User.Read` Değeri, oturum açmış olan kullanıcının temel profil okumanızı sağlar.

Kullanılabilir tüm kapsamların hakkında daha fazla bilgiyi [Microsoft Graph izin kapsamları](https://graph.microsoft.io/docs/authorization/permission_scopes).

İçin `authURL`, `loginURL`, `bhh`, ve `tokenURL`, daha önce sağlanan değerleri kullanmanız gerekir. Açık kaynak Microsoft Azure kimlik kitaplıklarını kullanırsanız, size bu verileri sizin için bizim meta veri uç noktasının kullanarak çekme. Bu değerleri sizin yerinize ayıklamak için çok çalıştık.

`keychain` değeri, NXOAuth2Client kitaplığının belirteçlerinizi depolamak üzere bir anahtarlık oluştururken kullanacağı kapsayıcıdır. Çoklu oturum açma (SSO) çok sayıda uygulamada almak istiyorsanız, aynı Anahtarlığı her uygulamalarınızı belirtin ve kullanılmasını, Xcode yetkilendirmeler isteyin. Bu Apple belgelerinde açıklanmıştır.

Bu değerlerin geri kalanı kitaplığı kullanmasına ve değerleri bağlama taşımak yer oluşturmak için gerekli.

### <a name="create-a-url-cache"></a>Bir URL önbelleği oluşturma
İçinde `(void)viewDidLoad()`, her zaman adlandırılan görünümü yüklendikten sonra aşağıdaki kodu bizim kullanmak için bir önbellek primes.

Aşağıdaki kodu ekleyin:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Oturum açma için bir Web görünümü oluşturma
Bir Web görünümü (yapılandırılmışsa) SMS mesajı gibi ek faktörler kullanıcıdan veya kullanıcıya hata iletileri döndürür. Burada ayarlarız Web görünümü ayarlanacak ve ardından kimlik hizmetlerini Webview'de gerçekleşecek geri çağırmaları işlemek üzere kod yazın.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Kimlik doğrulamasını işlemek için WebView yöntemlerini geçersiz kılın
Bir kullanıcı daha önce bahsedildiği gibi oturum açmak gerektiğinde ne olacağını WebView bildirmek için aşağıdaki kodu yapıştırabilirsiniz.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>OAuth2 isteğinin sonucunu işlemek için kod yazın
Aşağıdaki kod görünümünden döndürür Redirecturl'yi işleyecek. Kod, kimlik doğrulama başarısız olmadıysa yeniden deneyecek. Bu sırada, kitaplık, konsolda görebileceğiniz veya zaman uyumsuz olarak işlemek hata sağlar.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>OAuth (hesap deposu olarak adlandırılır) içeriğini ayarlama
Burada çağırabilirsiniz `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` erişebilmeleri için uygulamayı istediğiniz her hizmet için paylaşılan hesap deposu. Hesap türü için belirli bir hizmeti bir tanımlayıcı olarak kullanılan bir dizedir. Grafik API'sine erişim için kod olarak başvurduğu `"myGraphService"`. Ardından herhangi bir şey belirteciyle değiştirildiğinde bildirir bir gözlemci ayarlanır. Belirteci aldıktan sonra dönüş, kullanıcının yeniden `masterView`.

```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Arama ve grafik API'si kullanıcılardan görüntülemek için ana görünüm ayarlama
Kılavuzda döndürülen verileri görüntüleyen bir ana-View-Controller (MVC) uygulama bu kılavuz kapsamında değildir ve birçok çevrimiçi eğitim bir nasıl oluşturulacağını açıklar. Bu kod iskelet dosyasıdır. Ancak, bu MVC uygulamasındaki birkaç şey uğraşmanız gerekir:

* Bir kullanıcı bir şey arama alanına yazdığında müdahale
* Kılavuzda sonuçları görüntüleyebilmeniz için bir nesne veri geri MasterView sağlar.

Biz bu aşağıdaki gerçekleştirirsiniz.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Oturum açtınız olmadığını görmek için bir denetim ekleme
Kullanıcı, imzalı değilse olup olmadığını zaten bir belirteç önbellekte denetlemek akıllı olacak şekilde uygulama az yapar. Aksi durumda, oturum açmak için kullanıcının (link) yönlendir. Geri çağırma, bir görünüm yüklenirken eylemlerini gerçekleştirmek için en iyi yolu kullanmak için ise `viewDidLoad()` Apple bize sağlayan yöntemidir.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>Veri alındığında, Tablo görünümünde güncelleştir
Grafik API'si veri döndürdüğünde, verileri görüntülemeniz gerekir. Kolaylık olması için tabloyu güncelleştirmek için tüm kod aşağıdadır. Bu gibi durumlarda, doğru değerleri yalnızca MVC Demirbaş kodunuzda yapıştırabilirsiniz.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Birisi arama alanına yazdığında grafik API'sini aramak için bir yol sağlama
Kullanıcı bir arama kutusuna yazdığında, grafik API'sine shove gerekir. `GraphAPICaller` Aşağıdaki kodda oluşturacak, sınıf sunudan arama işlevselliği ayırır. Şimdilik, tüm arama karakterleri grafik API'sine akışları kod yazalım. Adlı bir yöntem sağlayarak bunu `lookupInGraph`, biz aramak istediğiniz dizeyi alır.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Grafik API'sine erişmek için bir yardımcı sınıfı yazma
Uygulamamız çekirdek budur. Burada rest Apple varsayılan MVC düzeninde kod ekleme ancak kullanıcı türleri olarak grafiği sorgulamak ve bu verileri döndürmek için kod yazma. Kodu ve ayrıntılı bir açıklama onu izler.

### <a name="create-a-new-objective-c-header-file"></a>Yeni bir Objective C üst bilgi dosyası oluştur
Dosya adı `GraphAPICaller.h`ve aşağıdaki kodu ekleyin.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Burada belirtilen yöntem bir dize alır ve bir completionBlock döndürür bakın. Tahmin şekilde bu completionBlock doldurulan verileri gerçek zamanlı kullanıcı aramalarını olarak nesneyi sağlayarak tablo güncelleştirir.

### <a name="create-a-new-objective-c-file"></a>Yeni bir Objective C dosyası oluşturma
Dosya adı `GraphAPICaller.m`ve aşağıdaki yöntemi ekleyin.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

Bu yöntem ayrıntılı aracılığıyla edelim.

Bu kod çekirdek bulunduğu `NXOAuth2Request`, kullandığı parametreler yöntemi settings.plist dosyasında zaten tanımlı.

Sağ grafik API çağrısı oluşturmak için ilk adımdır bakın. Aradığınız çünkü `/users`, bu sürüm ile birlikte grafik API'si kaynak ekleyerek belirtin. API geliştikçe değiştirebilirsiniz çünkü bunlar bir dış ayarları dosyasında koymak için mantıklıdır.

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Ardından, grafik API'si çağrısı de sağlanır parametrelerini belirtmeniz gerekir. Bu *çok önemli* tüm URI olmayan uyumlu karakterlerin çalışma zamanında temizlendiği çünkü parametreleri'ı kaynak uç koymayın. Tüm sorgu kodu parametre sağlanmalıdır.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Bu çağrı fark edebilirsiniz bir `convertParamsToDictionary` henüz yazmadınız yöntemi. Şimdi dosya sonunda şimdi yapın:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Ardından, kullanalım `NXOAuth2Request` verileri JSON biçiminde API'sinden geri almak için yöntemi.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Son olarak, verileri MasterViewController dönüş nasıl bakalım. Veri sıralanmış olarak döndürür ve seri durumdan ve MainViewController tüketebileceği bir nesnede yüklü olması gerekiyor. Bu amaç için çatıyı sahip bir `User.m/h` dosya bir kullanıcı nesnesi oluşturur. Bu kullanıcı nesnesi grafiği alınan bilgilerle doldurun.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Örneği çalıştırma
Çatıyı kullanılan veya birlikte gözden geçirme ve ardından uygulamanızı şimdi çalıştırmanız gerekir. Simulator başlatır ve **oturum** uygulamayı kullanmak için.

## <a name="get-security-updates-for-our-product"></a>Ürünümüz için güvenlik güncelleştirmelerini alma
Güvenlik olayları ziyaret ederek ortaya çıktığında, bildirimleri almanızı öneririz [güvenlik TechCenter](https://technet.microsoft.com/security/dd252948) ve güvenlik önerisi uyarılarına abone.

